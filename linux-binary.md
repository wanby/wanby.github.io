1) soft link
 * ln -s /usr/local/node-1.12.2323/bin/node /usr/local/bin/node
 * ln -s /usr/local/node-1.12.2323/bin/npm /usr/local/bin/npm

2) env path
 * su root
 * vim /etc/profile
 * add env:
  + PATH=$PATH:/usr/local/node-1.12.2323/bin
  export PATH USER LOGNAME MAIL HOSTNAME .....

 * source /etc/profile
 * echo $PATH
