![[Pasted image 20221016003731.png]]
![[Pasted image 20221016003743.png]]

## Файл Playbook
```yml
---
- name: user
  hosts: demo
  tasks:
	- name: Create user
	  user:
		name: riser
		state: present
	become: true
```

## Запуск Ansible с Playbook'ом
```bash
ansible-playbook -i hosts.ini user.yml -K 
```
