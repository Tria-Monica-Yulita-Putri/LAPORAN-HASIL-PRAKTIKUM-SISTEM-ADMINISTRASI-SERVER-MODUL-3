# **LAPORAN HASIL PRAKTIKUM SISTEM ADMINISTRASI SERVER MODUL 3**

#### Create subdomain dev.vm.local use ansible and there are some rules:

​	a) Use ansible

​	b) Use same lxc which is used by vm.local

​	c) The folder code must be in /var/www/html/dev/{nama_app}



* Create a new file sublaravel.yml in directory ansible/laravel

  ```markdown
  nano larav.yml
  ```

  

  ![1](https://user-images.githubusercontent.com/92940432/146402876-c88b7e4b-c998-4d59-bee0-82b005499550.png)

  

  ```markdown
  ---
  - hosts: all
    become : yes
    tasks:
      - name: install bind9 dan dnsutils
        apt:
         pkg:
           - bind9
           - dnsutils
  ```

  

  

  ![2](https://user-images.githubusercontent.com/92940432/146402882-07168670-6d7a-433a-b353-aab8c54e5e12.png)

  

* Run or install ansible playbook larav.yml

  ```markdown
  ansible-playbook -i hosts larav.yml -k
  ```

  

  ![3](https://user-images.githubusercontent.com/92940432/146402886-fa64b697-84bf-4b79-bd31-d9ee0e0ee28a.png)


  

* Create a new file config2.yml

  ```markdown
  nano config2.yml 
  ```

  

  ![4](https://user-images.githubusercontent.com/92940432/146402893-885e000a-b6d4-4b08-8b1a-8e270d326549.png)

  

  ```markdown
  ---
  - hosts: all
    become : yes
    tasks:
     - name: membuat direktori
       file:
        path: /var/www/html/dev/landing
        state: directory
     - name: copy file vm.local
       copy:
        src: /etc/bind/vm/vm.local
        dest: /var/www/html/dev/landing
     - name: mengganti konfigurasi
       replace:
        path: /var/www/html/dev/landing/vm.local
        regexp: 'www'
        replace: 'dev'
     - name: copy file named.conf.local
       copy:
        src: /etc/bind/named.conf.local
        dest: /etc/bind/named.conf.local
     - name: mengganti konfigurasi conf local
       replace:
        path: /etc/bind/named.conf.local
        regexp: '/etc/bind/vm/vm.local'
        replace: '/var/www/html/dev/landing/vm.local'
     - name: mengganti konfigurasi conf local part2
       replace:
        path: /etc/bind/named.conf.local
        regexp: '/etc/bind/vm/1.168.192.in-addr.arpa'
        replace: '/var/www/html/dev/landing/1.168.192.in-addr.arpa'
     - name: copy file 1.168.192.in-addr.arpa
       copy:
        src: /etc/bind/vm/1.168.192.in-addr.arpa
        dest: /var/www/html/dev/landing
     - name: copy file resolv.conf
       copy:
        src: /etc/resolv.conf
        dest: /etc/resolv.conf
     - name: copy file named.conf.options
       copy:
        src: /etc/bind/named.conf.options
        dest: /etc/bind/named.conf.options
     - name: restart nginx
       service:
        name: nginx
        state: restarted
     - name: restart bind9
       action: service name=bind9 state=restarted
  ```

  

  

  ![5](https://user-images.githubusercontent.com/92940432/146402894-2b36e5a2-9b61-4ba9-86de-3bbe272422f4.png))

  

* Run or install ansible playbook config2.yml

  ```markdown
  ansible-playbook -i hosts config2.yml -k
  ```

  

  ![6](https://user-images.githubusercontent.com/92940432/146402897-929d8697-18c2-4a9d-9818-5d73f0de5588.png)

  

* Add this in file hosts

  ```markdown
  dev.vm.local
  10.0.3.103	dev.vm.local	
  ```

  

  ![7](https://user-images.githubusercontent.com/92940432/146402903-f62707ca-06dd-4613-a93c-ddd063ab7b3f.png)


  

* Log in to root ubuntulanding then edit vm.local

  ```markdown
  ssh root@lxc_landing.dev
  ```

  

  ![8](https://user-images.githubusercontent.com/92940432/146402909-2607619f-8338-4b4a-a423-7868ab72c77f.png)

  

* Write in vm.local, then add line www

  ````markdown
  nano /var/www/html/dev/landing/vm.local
  ````

  

  ![9](https://user-images.githubusercontent.com/92940432/146402912-8ad3210d-105c-46d5-964d-69cf59226120.png)

  

* The next step is edit vm.local. Add dev.vm.local

  ```markdown
  nano /etc/nginx/sites-enabled/vm.local
  dev.vm.local
  ```

  

  
  ![10](https://user-images.githubusercontent.com/92940432/146402914-a47f6075-9c7e-4c72-8c37-645fe2aaa194.png)


  

* Edit file vm.local in directory bind. Add line dev

  
  
  ![11](https://user-images.githubusercontent.com/92940432/146402917-5ec2d570-135a-49d5-bc9e-1767eee2d9ec.png)
  
  
  
* For the last step, do a restart for all services.

  ```markdown
  sudo service bind9 restart
  sudo service nginx restart
  sudo /etc/init.d/named restart
  ```

  

  ![12](https://user-images.githubusercontent.com/92940432/146402919-96b4f710-da5c-467f-b5ca-b7f3c261b721.png)

  

* To check your configuration, open the browser and search for dev.vm.local and dev.vm.local/blog 

  

  ![13](https://user-images.githubusercontent.com/92940432/146402925-b76d2910-3d9b-439f-abf0-a1c8a83f750d.png)

  

  ![14](https://user-images.githubusercontent.com/92940432/146402930-7c992040-1cd9-495c-8237-da9e6000a32b.png)

  
  
  

