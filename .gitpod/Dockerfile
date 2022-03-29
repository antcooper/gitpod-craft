FROM gitpod/workspace-base

USER root

# Dazzle does not rebuild a layer until one of its lines are changed. Increase this counter to rebuild this layer.
ENV TRIGGER_REBUILD=1

# Install MySQL
RUN install-packages mysql-server \
 && mkdir -p /var/run/mysqld /var/log/mysql \
 && chown -R gitpod:gitpod /etc/mysql /var/run/mysqld /var/log/mysql /var/lib/mysql /var/lib/mysql-files /var/lib/mysql-keyring /var/lib/mysql-upgrade

# Install our own MySQL config
COPY .gitpod/mysql.cnf /etc/mysql/mysql.conf.d/mysqld.cnf

# Install default-login for MySQL clients
COPY .gitpod/client.cnf /etc/mysql/mysql.conf.d/client.cnf

COPY .gitpod/mysql-bashrc-launch.sh /etc/mysql/mysql-bashrc-launch.sh
COPY .gitpod/mysql-bashrc-launch.sh /home/gitpod/.bashrc.d/100-mysql-launch

USER gitpod

# Handle PHP install
RUN mkdir ~/.cache && /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
ENV PATH=/home/linuxbrew/.linuxbrew/bin:/home/linuxbrew/.linuxbrew/sbin/:$PATH
ENV MANPATH="$MANPATH:/home/linuxbrew/.linuxbrew/share/man"
ENV INFOPATH="$INFOPATH:/home/linuxbrew/.linuxbrew/share/info"
ENV HOMEBREW_NO_AUTO_UPDATE=1

RUN sudo apt remove -y cmake \
    && brew install cmake \
    && brew install php@8.0 \
    && brew install mysql

ENV PATH=/home/linuxbrew/.linuxbrew/opt/php@8.0/bin:/home/linuxbrew/.linuxbrew/opt/php@8.0/sbin:$PATH
