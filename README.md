Multi site yapısı ile revize edildi.

1.  Yapıyı docker compose up -d --build ile ayağa kaldır.

2.  docker-compose run composer create-project laravel/laravel <LaravelProjeAdı> "<LaravelVersion>.\*" --prefer-dist
    örn: docker-compose run composer create-project laravel/laravel my-site "11.0.\*" --prefer-dist
3.  dockerfiles klasöründe bulunan conf.d klasörünün içerisinde bulunan dosyayı kopyalayıp projenizi tanımlayan isimle tekrar kaydederek düzenleyin.
    örn:

        server {
        listen 80;
        server*name proje1.test; # Uygun bir domain yazın.
        index index.php index.html;
        server_name *;
        root /var/www/html/my-site/public; # www klasörünün altında oluşturulan yeni klasörünün public klasörünün yolunu ayarlayın.

          location / {
              try_files $uri $uri/ /index.php?$query_string;
          }

          location ~ \.php$ {
              try_files $uri =404;
              fastcgi_split_path_info ^(.+\.php)(/.+)$;
              fastcgi_pass php:9000;  ### bu kısım önemli burada php konteynerinizin adı yer alacak.
              fastcgi_index index.php;
              include fastcgi_params;
              fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
              fastcgi_param PATH_INFO $fastcgi_path_info;
          }

    }
