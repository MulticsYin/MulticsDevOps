# 使用 Golang 开发 Kafka 程序

使用 Golang 来开发 Kafka 可以使用 sarama 库。  
sarama 的 GitHub 链接: [https://github.com/Shopify/sarama](https://github.com/Shopify/sarama)  

生产者demo代码,可以不断输入字符串并作为message发送到topic中:  
```go
package main

import (
    "fmt"
    "github.com/Shopify/sarama"
)

func main() {
    config := sarama.NewConfig()
    config.Producer.RequiredAcks = sarama.WaitForAll
    config.Producer.Partitioner = sarama.NewRandomPartitioner

    producer, err := sarama.NewSyncProducer([]string{"localhost:9092"}, config)
    if err != nil {
        panic(err)
    }

    defer producer.Close()

    msg := &sarama.ProducerMessage {
        Topic: "kltao",
        Partition: int32(-1),
        Key:        sarama.StringEncoder("key"),
    }

    var value string
    for {
        _, err := fmt.Scanf("%s", &value)
        if err != nil {
            break
        }
        msg.Value = sarama.ByteEncoder(value)
        fmt.Println(value)

        partition, offset, err := producer.SendMessage(msg)
        if err != nil {
            fmt.Println("Send message Fail")
        }
        fmt.Printf("Partition = %d, offset=%d\n", partition, offset)
    }
}
```
