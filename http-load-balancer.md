# Create a HTTP(s) Load Balancer

````
gcloud compute http-health-checks create http-basic-check
````

Create a name-port ``http`` mapped to the port ``80``
````
gcloud compute instance-groups managed set-named-ports nginx-group --named-ports http:80
````       

Create a backend service:
````
gcloud compute backend-services create nginx-backend \
      --protocol HTTP --http-health-checks http-basic-check --global
````

Add the instance group into the backend service:
````
gcloud compute backend-services add-backend nginx-backend \
    --instance-group nginx-group \
    --instance-group-zone us-central1-a \
    --global
````

````
gcloud compute url-maps create web-map \
    --default-service nginx-backend
````

````
gcloud compute target-http-proxies create http-lb-proxy \
    --url-map web-map
````    

````
gcloud compute forwarding-rules create http-content-rule \
        --global \
        --target-http-proxy http-lb-proxy \
        --ports 80
````
