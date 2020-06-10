# Spark Image Building

### source
Most of content from: https://github.com/kubernetes/application-images/tree/master/spark
I changed initial image, because the original image is old when you run apt update, something wrong happened. So when you use this, maybe you need to udpate again.

### Build
```sh
docker build -t spark:2.4.5_v1 .
```
