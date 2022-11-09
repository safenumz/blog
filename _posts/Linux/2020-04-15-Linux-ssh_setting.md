---
layout: post
title: '[Linux] ssh 설정 스크립트'
category: Linux
tags: [linux, public key]
comments: true
---

# ssh 설정 스크립트
- Kubernetes, Kafka 등의 Cluster 학습을 할 때, public key 공유를 하기 번거로워 작성함
- 가상환경 구성 등의 간단한 실습용으로 실무에서 사용하려면 코드를 더 수정해야 함
- 각 SERVER, CLIENT에 ~/.ssh/authorized_keys 쓰기 권한이 있어야 함, 400으로 막혀있으면 안됨
- CLIENT가 있다면, 사전에 CLIENT에 sshpass가 설치되어 있어야 함

## ssh_config.sh
- CLIENT, SERVER 들의 USER_NAME, IP, Password, PORT, HOSTNAME 정보 설정

```bash
#!/bin/bash
# ssh_config.sh

# CLIENT
# CLIENT 서버가 있다면 사전에 sshpass가 설치되어 있어야 함
# Available OS: CentOS, Ubuntu, Mac
# CLIENT=(user  ip  password  port nickname)
CLIENT=(a 192.168.0.99 <password> 22 lab)

# SERVER
# Available OS: CentOS, Ubuntu
# SERVER=(user  ip  password  port nickname)
SERVER_1=(root 192.168.0.100 <password> 22 lab100)
SERVER_2=(root 192.168.0.101 <password> 22 lab101)
SERVER_3=(root 192.168.0.102 <password> 22 lab102)
SERVER_4=(root 192.168.0.103 <password> 22 lab103)

SERVER_LIST=(
    SERVER_1[@]
    SERVER_2[@]
	SERVER_3[@]
	SERVER_4[@]
)
```

## setup_ssh.sh
- 각 서버들에 sshpass 설치, ssh-keygen, public key 공유

```bash
#!/bin/bash
# ===========================================================================
# Program: setup_ssh.sh
#    Path: .
#   Usage: bash setup_ssh.sh $1
#      $1: $PWD/ssh_config.sh 
# ============================================================================

source $1

function remote_cmd() {
	local user=$1
	local ip=$2
	local passwd=$3
	local port=$4
	local command=$5
	sshpass -p ${passwd} ssh -p ${port} -o StrictHostKeyChecking=no ${user}@${ip} "export PATH=$PATH:/usr/local/bin:/usr/bin && ${command}"
}

function install_sshpass() {
	local user=$1
	local ip=$2
	local passwd=$3
	local port=$4

	echo "*** install-sshpass ${ip}:${port}"

	if [[ $(remote_cmd "$user" "$ip" "$passwd" "$port" "cat /etc/*release") =~ "CentOS" ]]; then
		remote_cmd "$user" "$ip" "$passwd" "$port" "yum install -y sshpass"
	elif [[ $(remote_cmd "$user" "$ip" "$passwd" "$port" "cat /etc/*release") =~ "Ubuntu" ]]; then
		remote_cmd "$user" "$ip" "$passwd" "$port" "apt-get install -y sshpass"
	else
		echo "[ERROR] Invalid OS... (Valid OS: CentOS, Ubuntu)"
		exit 1
	fi
}

function ssh_keygen() {
	local user=$1
	local ip=$2
	local passwd=$3
	local port=$4
	local nickname=$5
	
	echo "*** ssh-keygen ${nickname}(${user}@${ip}:${port})"

	remote_cmd "$user" "$ip" "$passwd" "$port" "ssh-keygen -t rsa -P '' -f ~/.ssh/id_rsa &&  cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys" <<< y > /dev/null

}

function share_ssh_key() {
	local src_user=$1
	local src_ip=$2
	local src_passwd=$3
	local src_port=$4
	local src_nickname=$5
	local dst_user=$6
	local dst_ip=$7
	local dst_passwd=$8
	local dst_port=$9
	local dst_nickname=$10
	local dst_home=$(remote_cmd "$dst_user" "$dst_ip" "$dst_passwd" "$dst_port" "echo \$HOME")
	
	echo "*** share ssh-key ${src_nickname}(${src_user}@${src_ip}:${src_port}) -> ${dst_nickname}(${dst_user}@${dst_ip}:${dst_port})"
	echo "sshpass -p ${dst_passwd} scp -P ${dst_port} -o StrictHostKeyChecking=no ~/.ssh/id_rsa.pub ${dst_user}@${dst_ip}:/${dst_home}/.ssh/${src_nickname}.pub"

	remote_cmd "$src_user" "$src_ip" "$src_passwd" "$src_port" "sshpass -p ${dst_passwd} scp -P ${dst_port} -o StrictHostKeyChecking=no ~/.ssh/id_rsa.pub ${dst_user}@${dst_ip}:/${dst_home}/.ssh/${src_nickname}.pub"

	remote_cmd "$dst_user" "$dst_ip" "$dst_passwd" "$dst_port" "cat ~/.ssh/${src_nickname}.pub >> ~/.ssh/authorized_keys"
}

function set_authority() {
	local user=$1
	local ip=$2
	local passwd=$3
	local port=$4
	local nickname=$5

	echo "*** set authority ${nickname}(${user}@${ip}:${port})"

	remote_cmd "$user" "$ip" "$passwd" "$port" "chmod 400 ~/.ssh/authorized_keys"
}

function validate_ssh_conn() {
	local src_user=$1
	local src_ip=$2
	local src_passwd=$3
	local src_port=$4
	local src_nickname=$5
	local dst_user=$6
	local dst_ip=$7
	local dst_port=$8
	local dst_nickname=$9
	
	echo -e "\n\n*** validate ssh-connection ${src_nickname}(${src_user}@${src_ip}:${src_port}) -> ${dst_nickname}(${dst_user}@${dst_ip}:${dst_port})"

	remote_cmd "$src_user" "$src_ip" "$src_passwd" "$src_port" "ssh ${dst_user}@${dst_ip} -p ${dst_port} 'echo 2>&1' && echo '[SSH CONNECTION SUCCESS] ${src_nickname}(${src_user}@${src_ip}:${src_port}) -> ${dst_nickname}(${dst_user}@${dst_ip}:${dst_port})' || echo '[SSH CONNECTION FAILURE] ${src_nickname}(${src_user}@${src_ip}:${src_port}) -> ${dst_user}@${dst_nickname}(${dst_ip}:${dst_port})'"

}

function main() {
	# client
	if [ ! -z $CLIENT ]; then
		ssh-keygen -t rsa -P '' -f ~/.ssh/id_rsa <<< y > /dev/null
		cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
		chmod 400 ~/.ssh/authorized_keys
	fi

	# server: sshpass 설치 및 ssh-keygen
	for ((i=0; i<${#SERVER_LIST[@]}; i++))
	do
		local user=${!SERVER_LIST[i]:0:1}
		local ip=${!SERVER_LIST[i]:1:1}
		local passwd=${!SERVER_LIST[i]:2:1}
		local port=${!SERVER_LIST[i]:3:1}
		local nickname=${!SERVER_LIST[i]:4:1}
		# sshpass 설치
		install_sshpass "$user" "$ip" "$passwd" "$port"
		# ssh-keygen
		ssh_keygen "$user" "$ip" "$passwd" "$port" "$nickname"
	done

	# server: 각 서버 간 public key 공유
	for ((i=0; i<${#SERVER_LIST[@]}; i++))
	do
		local src_user=${!SERVER_LIST[i]:0:1}
		local src_ip=${!SERVER_LIST[i]:1:1}
		local src_passwd=${!SERVER_LIST[i]:2:1}
		local src_port=${!SERVER_LIST[i]:3:1}
		local src_nickname=${!SERVER_LIST[i]:4:1}
		
		# client
		if [ ! -z $CLIENT ]; then
			local client_nickname=${CLIENT[4]}
			sshpass -p "$src_passwd" scp -P "$src_port" -o StrictHostKeyChecking=no ~/.ssh/id_rsa.pub "${src_user}"@"${src_ip}":/$(echo \$HOME)/.ssh/${client_nickname}.pub
			remote_cmd "$src_user" "$src_ip" "$src_passwd" "$src_port" "cat ~/.ssh/${client_nickname}.pub >> ~/.ssh/authorized_keys"
		fi

		for ((j=0; j<${#SERVER_LIST[@]}; j++))
		do
			local dst_user=${!SERVER_LIST[j]:0:1}
			local dst_ip=${!SERVER_LIST[j]:1:1}
			local dst_passwd=${!SERVER_LIST[j]:2:1}
			local dst_port=${!SERVER_LIST[j]:3:1}
			local dst_nickname=${!SERVER_LIST[j]:4:1}
			if [[ "${src_ip}:${src_port}" != "${dst_ip}:${dst_port}" ]]; then
				share_ssh_key "$src_user" "$src_ip" "$src_passwd" "$src_port" "$src_nickname" "$dst_user" "$dst_ip" "$dst_passwd" "$dst_port" "$dst_nickname"
			fi
		done
	done

	# authorized_keys 보안 설정
	for ((i=0; i<${#SERVER_LIST[@]}; i++))
	do
		local user=${!SERVER_LIST[i]:0:1}
		local ip=${!SERVER_LIST[i]:1:1}
		local passwd=${!SERVER_LIST[i]:2:1}
		local port=${!SERVER_LIST[i]:3:1}
		local nickname=${!SERVER_LIST[i]:4:1}

		set_authority "$user" "$ip" "$passwd" "$port" "$nickname"
	done

	# 패스워드없이 잘 접속되는지 최종 확인
	for ((i=0; i<${#SERVER_LIST[@]}; i++))
	do
		local src_user=${!SERVER_LIST[i]:0:1}
		local src_ip=${!SERVER_LIST[i]:1:1}
		local src_passwd=${!SERVER_LIST[i]:2:1}
		local src_port=${!SERVER_LIST[i]:3:1}
		local src_nickname=${!SERVER_LIST[i]:4:1}

		for ((j=0; j<${#SERVER_LIST[@]}; j++))
		do
			local dst_user=${!SERVER_LIST[j]:0:1}
			local dst_ip=${!SERVER_LIST[j]:1:1}
			local dst_port=${!SERVER_LIST[j]:3:1}
			local dst_nickname=${!SERVER_LIST[j]:4:1}

			if [[ "${src_ip}:${src_port}" != "${dst_ip}:${dst_port}" ]]; then
				validate_ssh_conn "$src_user" "$src_ip" "$src_passwd" "$src_port" "$src_nickname" "$dst_user" "$dst_ip" "$dst_port" "$dst_nickname"
			fi
		done
	done
}


main
```