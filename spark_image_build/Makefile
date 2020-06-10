all: spark
push: push-spark 
.PHONY: push push-spark spark 

# Change to your repository(I used docker hub)
TAG = 2.4.5_v1

spark:
	docker build -t spark:$(TAG).
	docker tag spark:2.4.5_v1 kainvey/spark:$(TAG)

# Push to repository
push-spark: spark
	gcloud docker push kainvey/spark
	gcloud docker push kainvey/spark/spark:$(TAG)

clean:
	docker rmi kainvey/spark:$(TAG) || :
	docker rmi kainvey/spark

	
