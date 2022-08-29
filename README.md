# setup-dev-machine-ubuntu-
```bash
#instalar curl
sudo apt install curl
curl -sL https://deb.nodesource.com/setup_lts.x | sudo -E bash -
curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add -
echo "deb https://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list

sudo apt-get update
sudo apt-get install git-core zlib1g-dev build-essential libssl-dev libreadline-dev libyaml-dev libsqlite3-dev sqlite3 libxml2-dev libxslt1-dev libcurl4-openssl-dev software-properties-common libffi-dev nodejs yarn
```

```bash
#instalar rvm
curl -sSL https://rvm.io/mpapis.asc | gpg --import -
curl -sSL https://rvm.io/pkuczynski.asc | gpg --import -
curl -sSL https://get.rvm.io | bash -s stable

sudo apt-get install git-core zlib1g-dev build-essential libssl-dev libreadline-dev libyaml-dev libsqlite3-dev sqlite3 libxml2-dev libxslt1-dev libcurl4-openssl-dev software-properties-common libffi-dev
sudo apt install curl g++ gcc autoconf automake bison libc6-dev libffi-dev libgdbm-dev libncurses5-dev libsqlite3-dev libtool libyaml-dev make pkg-config sqlite3 zlib1g-dev libgmp-dev libreadline-dev libssl-dev

sudo apt-get install rvm

sudo nano /etc/apt/sources.list
add deb http://security.ubuntu.com/ubuntu bionic-security main
sudo apt update && apt-cache policy libssl1.0-dev
sudo apt-get install libpq-dev
sudo apt-get install libssl1.0-dev
```
```bash
bundle install no projeto
```
```bash
#instalar postgres
wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -
echo "deb http://apt.postgresql.org/pub/repos/apt/ $(lsb_release -cs)-pgdg main" | sudo tee /etc/apt/sources.list.d/postgresql-pgdg.list > /dev/null
sudo apt-get update -y
sudo apt install postgresql-9.6 postgresql-client-9.6

#tem que criar o user fora do psql
sudo -u postgres createuser chris -s
sudo -u postgres psql
\password dev

sudo su
cd /etc/postgresql
cd 9.6/main
sudo nano pg_hba.conf
- trocar de peer para trust
sudo nano postgresql.conf

- descomentar listen_addresses e deixar igual abaixo.
listen_addresses = '0.0.0.0'

service postgresql restart
#abaixo precisa estar running
service postgresql status

```
```bash
echo "Instalando Redis 3.2"
adduser --system --group --no-create-home redis
mv arquivos/redis/redis-3.2.9.tar.gz /tmp
cd /tmp
tar xzvf redis-3.2.9.tar.gz
cd /tmp/redis-3.2.9
make
make test
sudo make install
cd
mkdir /etc/redis
mv arquivos/redis/redis.conf /etc/redis
cat << EOF > /etc/systemd/system/redis.service
  [Unit]
  Description=Redis In-Memory Data Store
  After=network.target
  [Service]
  User=redis
  Group=redis
  ExecStart=/usr/local/bin/redis-server /etc/redis/redis.conf
  ExecStop=/usr/local/bin/redis-cli shutdown
  Restart=always
  [Install]
  WantedBy=multi-user.target
EOF
mkdir /var/lib/redis
chown redis:redis /var/lib/redis
chmod 770 /var/lib/redis
systemctl start redis
systemctl enable redis
```



Projeto Pitzi

