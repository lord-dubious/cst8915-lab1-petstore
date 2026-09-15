# CST8915 Lab 1 demo plan

Target length: 3 to 4 minutes. Maximum permitted length: 5 minutes.

## 1. Introduction and Azure evidence, 20 seconds

Say:

> This is my CST8915 Lab 1 deployment of the Algonquin Pet Store on an Azure virtual machine. The VM runs Ubuntu 24.04 on an x64 Standard B2als v2 instance with 2 virtual CPUs and 4 GiB of memory.

Show:

- Azure VM Overview with the VM name, running state, operating system, size, and public IP.
- Do not reveal the private SSH key or any credentials.

## 2. Services and architecture, 40 seconds

Say:

> The system contains a Vue store front, a Rust product service, a Node.js order service, and RabbitMQ. The product service supplies the catalogue. The order service accepts orders and publishes them to a durable RabbitMQ queue. RabbitMQ decouples order intake from future order processing.

Show in an SSH terminal:

```bash
systemctl is-active rabbitmq-server
ss -ltn | grep -E ':(3000|3030|8080) '
```

Point out that RabbitMQ is active and the three application ports are listening.

## 3. Product Service, 30 seconds

Say:

> The Rust Product Service uses Warp and Tokio. Its GET products endpoint runs on port 3030 and returns the product catalogue as JSON.

Show:

```bash
curl http://localhost:3030/products
```

## 4. Store Front and order workflow, 60 seconds

Say:

> The Vue store front loads the products from the Product Service. I will select Dog Food, enter a quantity of two, and confirm that the total is $39.98.

Show:

- Open `http://<VM-PUBLIC-IP>:8080`.
- Select Dog Food.
- Enter quantity `2`.
- Pause briefly on `Total Price: $39.98`.
- Click **Place Order**.
- Show the success message.

## 5. RabbitMQ verification, 35 seconds

Say:

> The Node.js Order Service accepts the JSON order on port 3000 and publishes a persistent message to RabbitMQ. The durable order queue now contains the submitted order. The lab does not include a consumer, so successful orders remain queued.

Show:

```bash
sudo rabbitmqctl list_queues name durable messages
tail -n 5 ~/order-service.log
```

Point out `order_queue`, `true`, and the increased message count.

## 6. Closing, 20 seconds

Say:

> This demonstration confirms that the VM-hosted Store Front, Product Service, Order Service, and RabbitMQ broker work together successfully. After saving the required evidence and submission files, I will delete the dedicated Azure resource group to prevent additional credit usage.

## Recording checklist

- [ ] Recording is no longer than five minutes.
- [ ] Azure VM details are visible without exposing credentials.
- [ ] Browser uses the VM public-IP URL.
- [ ] Products load.
- [ ] Two Dog Food units total $39.98.
- [ ] Order success message appears.
- [ ] RabbitMQ durable queue and increased message count are visible.
- [ ] Video is reviewed before upload.
- [ ] YouTube visibility is Unlisted, not Private.
