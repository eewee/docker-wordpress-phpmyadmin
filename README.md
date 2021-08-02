# Docker-composer pour Wordpress :
Le but étant de pouvoir utiliser, les images suivantes avec Docker :
* 1 Wordpress
* 1 db
* 1 PhpMyAdmin
* 1 MailDev

# Config :
* On se place dans le dossier ne contenant que le fichier "docker-compose.yml".
* On lance un `docker-compose up -d`.
* Tapez maintenant l'url suivante :
  * [localhost](http://localhost), pour accéder à la page d’installation de WordPress.
  * [localhost:8080](http://localhost:8080), pour accéder à PhpMyAdmin.
  * [localhost:1080](http://localhost:1080), pour accéder à MailDev.

# SwiftMail : 

* `cd /wp-content/themes/twentytwentyone`
* `composer init`
* `composer require "swiftmailer/swiftmailer:^6.0" --dev`
* `composer install`

Edit > function.php :
```
function fct_shoot_email() {
    require_once 'vendor/autoload.php';

    // Create the Transport
    $transport = (new Swift_SmtpTransport('maildev', 25))
      ->setUsername('')
      ->setPassword('')
    ;

    // Create the Mailer using your created Transport
    $mailer = new Swift_Mailer($transport);

    // Create a message
    $message = (new Swift_Message('Mon sujet'))
      ->setFrom(['john@doe.com' => 'John DOE'])
      ->setTo(['foo@bar.com' => 'Foo BAR'])
      ->setBody('Lorem ipsum')
      ->addPart('<b>Lorem ipsum</b>', 'text/html')
      ;

    // Send the message
    $result = $mailer->send($message);
}
add_action( 'wp_footer', 'fct_shoot_email' );
```

# Source :
* https://www.eewee.fr/docker-comme-environnement-de-developpement-ca-dechire/
