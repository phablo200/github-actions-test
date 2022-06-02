# Test run container
sudo docker run \
    --name mytest-01 \
    node

sudo docker container ps | grep "mytest-01"
sudo docker container ls --all | grep "mytest-01"

sudo docker container rm -f mytest-01

sudo docker run \
    --name mytest-01 \
    -it --entrypoint /bin/bash echo "Hello World" \
    node



