# devops
End to End Automated DevOps Solution with PetClinic
--Install Docker
sudo apt-get update
sudo apt-get install \
    ca-certificates \
    curl \
    gnupg \
    lsb-release
sudo mkdir -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin
sudo usermod -aG docker ${USER}
--Install docker-compose
sudo apt  install docker-compose -y
--Install network tools
sudo apt install net-tools
--Install Java 17 ---
sudo apt install software-properties-common
sudo add-apt-repository ppa:linuxuprising/java -y
sudo apt update
sudo apt install oracle-java17-installer --install-recommends
java -version
-- Run petclinic code
Clone git repo
Copy key files to .ssh
chmod 400 gk
mv gk .ssh/
export GIT_SSH_COMMAND='ssh -i /home/ubuntu/.ssh/gk'
git clone git@github.com:gkolluru/devops.git
cd devops/spring-petclinic
docker build . -t petc:local
-- Create directories to hold mysql data
mkdir petdata
mkdir mysql_config
-- Finally Run using
docker-compose up -d
-- Verify Data from backend
docker run -it --rm --network spring-petclinic_mynet mysql mysql -h mysql -upetuser -p
use petdb;
select first_name from owners;
------
--Alter nate repo & commands
git clone https://github.com/dockersamples/spring-petclinic-docker.git
./mvnw spring-boot:run

docker run -p 8080:8080 -e "POSTGRES_URL=jdbc:postgresql://spring-petclinic_postgres_1/petclinic" petc:local ./mvnw spring-boot:run -Dspring.profiles.active=postgres

docker run -p 8080:8080 -e "MYSQL_URL=jdbc:mysql://spring-petclinic_mysql_1/petclinic" petc:local "./mvnw spring-boot:run -Dspring.profiles.active=mysql"

docker run -it --rm --network spring-petclinic_default -v $PWD/pet_data/:/var/lib/postgresql/data postgres:15.1 psql -h spring-petclinic_postgres_1 -U petclinic
