<div id="top"></div>

<br />
<div align="center">
  <h1 align="center">Kafka</h1>
</div>

## Kafka vs Rabbit

|                    |                      Kafka                       |        Rabbit        |
| :----------------: | :----------------------------------------------: | :------------------: |
|     Same Order     |                        Có                        |        Không         |
|    Scalability     |      Tăng số partition và instance consumer      |                      |
| Dead letter queue  |                      Không                       |          Có          |
|     Deployment     |                Khó kiểm soát hơn                 |   Lightweight hơn    |
|    Commit(ack)     | Dựa vào message cuối cùng nhận được trong batch. | Từng message độc lập |
| Publish dạng batch |                        Có                        |        Không         |
|   Resume/ Pause    |                        Có                        |        Không         |

## Main concepts.

### 1. Subscribed.

- N instance chung `groupId` (chung `consumer-group`) consume đồng thời vào một topic có M partition. Thì số lượng message nhận được ở mỗi instance phụ thuộc vào mối tương quan giữa N và M, [xem thêm.](https://sagarkudu.medium.com/explain-consumer-group-in-kafka-1c61fa3a77b)
- 2 instance chung `groupId` (chung `consumer-group`) không thể consume đồng thời 2 topic tai 1 thời điểm, [xem thêm](https://github.com/tulios/kafkajs/issues/1040)
- Số message lấy được dạng batch, ta có thể config lại size của batch này nhưng tính theo đơn vị KB chứ không phải số lượng.
- Trong hàm handle message (`eachMessage`, `eachBatch`), nếu bị throw lỗi thì sẽ consume lại message / batch đó.
- Cẩn thận với heatbeat, heartbeat nếu set là 10s, thì sau 10s mà chưa heartbeat, thì sẽ bị lỗi, đánh dấu lần publish nó fail. Hình như không gọi là tự gọi.

### 2.Publish

- Khi publish dữ liệu vô kafka, cần quan tâm **key** của message, **key** này sẽ được băm `murmur2(key) % number_of_partitions` để xác định đi vào partition nào. Nếu không có key thì message sẽ được load balance đều cho các partition, [xem thêm.](https://forum.confluent.io/t/what-should-i-use-as-the-key-for-my-kafka-message/312)
- Giả sử ta có 2 event của một đơn hàng (create - delete), ta phải set **key** cho 2 message chung để cho chúng nó vào chung một partition. Từ đó 2 event sẽ giữ đúng thứ tự, tránh việc event delete đến trước, create đến sau.
- Header thì hình như có đủ thông tin vừa đủ để check ABC mà không cần parse message ra để check.
- Với một topic có thể được đẩy dữ liệu từ nhiều producer khác nhau bởi những nguyên nhân sau:
  - Một topic mà có nhiều format data
  - Hai process khác nhau cùng đẩy data vô. Nếu dùng chung 1 producer sẽ lỗi `write after end`
- Send multi topic có tốc độ nhanh hơn send rời rạc từng topic.

  <p align="right">(<a href="#top">Back to top</a>)</p>
