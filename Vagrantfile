Vagrant.configure("2") do |config|
  config.vm.define "docker_machine" do |docker_machine|
    docker_machine.vm.box = "kwilczynski/ubuntu-20.04-docker"
    docker_machine.vm.network "private_network", ip: "192.168.56.0"
    docker_machine.vm.network "forwarded_port", guest: 5000, host: 5002, id:"dock" 
    docker_machine.vm.provision "shell", inline: <<-SHELL
    apt-get update
    CURRENT_WORKDIR=$(pwd)
    FILE_ID="1EH1zS1wQ_5WmvUEar2eXcB4QMj1Mo49g"
    FILENAME="docker-compose-example.tgz"
    mkdir example && cd example
    wget --no-check-certificate "https://docs.google.com/uc?export=download&id=${FILE_ID}" -O ${FILENAME}
    tar xfz ${FILENAME}
    cd ${CURRENT_WORKDIR}
    cd example && docker-compose up
    SHELL
  end

  config.vm.define "apache_machine" do |apache_machine|
    apache_machine.vm.box = "ubuntu/trusty64"
    apache_machine.vm.network "private_network", ip: "192.168.56.1"
    apache_machine.vm.network "forwarded_port", guest: 80, host: 8082, id: "apa" 
    apache_machine.vm.provision "shell", inline: <<-SHELL
    apt-get update 
    apt-get install -y apache2
    sudo ufw allow 'Apache'
    sudo service apache2 start
    sudo ufw status
    SHELL
  end


  config.vm.define "nginx_machine" do |nginx_machine|
    nginx_machine.vm.box = "obihann/nginx"
    nginx_machine.vm.network "private_network", ip: "192.168.56.2"
    nginx_machine.vm.network "forwarded_port", guest: 80, host: 8080, id: "ng"
  end
end

