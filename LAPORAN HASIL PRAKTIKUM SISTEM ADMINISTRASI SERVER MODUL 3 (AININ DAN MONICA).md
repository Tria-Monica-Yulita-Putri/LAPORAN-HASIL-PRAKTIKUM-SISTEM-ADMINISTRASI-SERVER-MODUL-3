# **LAPORAN HASIL PRAKTIKUM SISTEM ADMINISTRASI SERVER MODUL 3**

#### Create subdomain dev.vm.local use ansible and there are some rules:

​	a) Use ansible

​	b) Use same lxc which is used by vm.local

​	c) The folder code must be in /var/www/html/dev/{nama_app}



* Create a new file sublaravel.yml in directory ansible/laravel

  ```markdown
  nano larav.yml
  ```

  

  ![](C:\Users\TRIA YULITA\OneDrive\Gambar\SAS MODUL 3\1.png)

  

  ![](C:\Users\TRIA YULITA\OneDrive\Gambar\SAS MODUL 3\2.png)

  

* Run or install ansible playbook larav.yml

  ```markdown
  ansible-playbook -i hosts larav.yml -k
  ```

  

  ![](C:\Users\TRIA YULITA\OneDrive\Gambar\SAS MODUL 3\3.png)

  

* Create a new file config2.yml

  ```markdown
  nano config2.yml 
  ```

  

  ![](C:\Users\TRIA YULITA\OneDrive\Gambar\SAS MODUL 3\4.png)

  

  ![](C:\Users\TRIA YULITA\OneDrive\Gambar\SAS MODUL 3\5.png)

  

* Run or install ansible playbook config2.yml

  ```markdown
  ansible-playbook -i hosts config2.yml -k
  ```

  

  ![](C:\Users\TRIA YULITA\OneDrive\Gambar\SAS MODUL 3\6.png)

  

* Add this in file hosts

  ```markdown
  dev.vm.local
  10.0.3.103	dev.vm.local	
  ```

  

  ![](C:\Users\TRIA YULITA\OneDrive\Gambar\SAS MODUL 3\7.png)

  

* Log in to root ubuntulanding then edit vm.local

  ```markdown
  ssh root@lxc_landing.dev
  ```

  

  ![](C:\Users\TRIA YULITA\OneDrive\Gambar\SAS MODUL 3\8.png)

  

* Write in vm.local, then add line www

  ````markdown
  nano /var/www/html/dev/landing/vm.local
  ````

  

  ![](C:\Users\TRIA YULITA\OneDrive\Gambar\SAS MODUL 3\9.png)

  

* The next step is edit vm.local. Add dev.vm.local

  ```markdown
  nano /etc/nginx/sites-enabled/vm.local
  dev.vm.local
  ```

  

  ![](C:\Users\TRIA YULITA\OneDrive\Gambar\SAS MODUL 3\10.png)

  

* Edit file vm.local in directory bind. Add line dev

  
  
  ![](C:\Users\TRIA YULITA\OneDrive\Gambar\SAS MODUL 3\11.png)
  
  
  
* For the last step, do a restart for all services.

  ```markdown
  sudo service bind9 restart
  sudo service nginx restart
  sudo /etc/init.d/named restart
  ```

  

  ![](C:\Users\TRIA YULITA\OneDrive\Gambar\SAS MODUL 3\12.png)

  

* To check your configuration, open the browser and search for dev.vm.local and dev.vm.local/blog 

  

  ![](C:\Users\TRIA YULITA\OneDrive\Gambar\SAS MODUL 3\13.png)

  

  ![](C:\Users\TRIA YULITA\OneDrive\Gambar\SAS MODUL 3\14.png)

  
  
  

