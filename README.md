# docker-vagrant-single
basig template for a vagrant machine installing the docker service

1. clone this project
2. customize Vagrantfile
3. customize ansible/playbook.yaml
4. run

```
vagrant box update
vagrant up
```

the content inside docker-images will be mounted into /home/ubuntu/docker-images
