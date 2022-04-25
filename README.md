# GCP_Homework4
# Creacion VMs
## Comando para crear la VM 1
    gcloud compute instances create vmrauqoz1 \
        --image-family ubuntu-pro-1804-lts \
        --image-project ubuntu-os-pro-cloud \
        --tags rauqozgcp \
        --metadata startup-script="#! /bin/bash
            sudo apt update -y; 
            sudo apt install docker.io -y; 
            sudo chmod 666 /var/run/docker.sock;

            docker run -d -p 80:3000 --name gcp4hw rauqoz/gcp4hw:amd64
        "
## Comando para crear la VM 2
    gcloud compute instances create vmrauqoz2 \
        --image-family ubuntu-pro-1804-lts \
        --image-project ubuntu-os-pro-cloud \
        --tags rauqozgcp \
        --metadata startup-script="#! /bin/bash
            sudo apt update -y; 
            sudo apt install docker.io -y; 
            sudo chmod 666 /var/run/docker.sock; 

            docker run -d -p 80:3000 --name gcp4hw rauqoz/gcp4hw:amd64
        "
## Comando para crear la VM 3
    gcloud compute instances create vmrauqoz3 \
        --image-family ubuntu-pro-1804-lts \
        --image-project ubuntu-os-pro-cloud \
        --tags rauqozgcp \
        --metadata startup-script="#! /bin/bash
            sudo apt update -y; 
            sudo apt install docker.io -y; 
            sudo chmod 666 /var/run/docker.sock; 

            docker run -d -p 80:3000 --name gcp4hw rauqoz/gcp4hw:amd64
        "
## Listado de VM creadas
    gcloud compute instances list
## Creando regla en el firewall -> Puerto 80
    gcloud compute firewall-rules create rauqozfw \
        --target-tags rauqozgcp \
        --allow tcp:80
# Crear LoadBalancer
## Creacion del load balancer...
    gcloud compute addresses create rauqozlb
## Agregando un health-check
    gcloud compute http-health-checks create rauqozhc
## Creacion de un target pool con el health check
    gcloud compute target-pools create rauqoztp \
        --http-health-check rauqozhc
## Agregar las instancias al target pool
    gcloud compute target-pools add-instances rauqoztp \
        --instances vmrauqoz1,vmrauqoz2,vmrauqoz3
## Agregando una regla de entrada
    gcloud compute forwarding-rules create rauqozre \
        --ports 80 \
        --address rauqozlb \
        --target-pool rauqoztp
## Revisando la informacion del load balancer
    gcloud compute forwarding-rules describe rauqozre
