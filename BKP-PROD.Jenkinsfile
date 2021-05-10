def secrets = [
    [password: pass, var: 'SECRET']
]

node() {
    wrap([$class: 'MaskPasswordsBuildWrapper', varPasswordPairs: secrets]) {
        def remote = [:]
  remote.name = 'P-TK-Jumpbox-01'
    remote.host = '172.30.210.194'
    remote.user = username
    remote.password = pass
    remote.allowAnyHosts = true


    stage('Limpando Workspace') {

     sshCommand remote: remote, command: "echo "+remote.password+" | sudo -S rm -rf  /home/"+username+"/jenkins/"
  }
    }
}

	pipeline {
        agent none

       
        stages {
        
            stage('Transferindo arquivos para Jumpbox'){
              steps {
                build 'P-TransferWorkspace' 
                }
            }

                  
            }
            }


node() {
    wrap([$class: 'MaskPasswordsBuildWrapper', varPasswordPairs: secrets]) {
        def remote = [:]
  remote.name = 'P-TK-Jumpbox-01'
    remote.host = '172.30.210.194'
    remote.user = username
    remote.password = pass
    remote.allowAnyHosts = true


    stage('Preparando os arquivos Ansible') {

     sshCommand remote: remote, command: "echo "+remote.password+" | sudo -S cp /home/"+username+"/jenkins/Ansible/all_hosts /etc/ansible/all_hosts"
     sshCommand remote: remote, command: "echo "+remote.password+" | sudo -S cp /home/"+username+"/jenkins/Ansible/playbooks/* /etc/ansible/playbooks/"
     

  }
stage('Adicionando Permissoes') {
sshCommand remote: remote, command: "echo "+pass+" | sudo -S ansible-playbook -b -u "+username+" /etc/ansible/playbooks/CHANGE-Permissions.yml -i /etc/ansible/all_hosts -e 'ansible_ssh_pass="+remote.password+" ansible_sudo_pass="+remote.password+"'"

  }
  
  stage('Realizando backup') {
    sshCommand remote: remote, command: "ansible-playbook -b -u "+username+" /etc/ansible/playbooks/P-BKP-Deploys-Properties.yml -i /etc/ansible/all_hosts -e 'ansible_ssh_pass="+remote.password+" ansible_sudo_pass="+remote.password+"'"

  }
    }
}

