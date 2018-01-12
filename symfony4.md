# =========================================
# =       Feature Symphony 3.3 -> 4       =
# =========================================

#Source BETA de Symfony 4 -> symfony/skeleton:^3.3
Require PHP-7.*
composer create-project "symfony/skeleton:^3.3" demo
cd demo

run `make serve`
Browse to the http://localhost:8000

#Répertoire
—————————————
your-project/
├── Makefile
├── config/
│   ├── bundles.php
│   ├── packages/
│   ├── routes.yaml
│   └── services.yaml
├── public/
│   └── index.php
├── src/
│   ├── ...
│   └── Kernel.php
├── templates/
└── vendor/

bin/
config/
public/
src/
var/
vendor/
Makefile
composer.lock
composer.json
.env
.env.dist
.gitignore

#Package composer
`req` est le raccourci de `require`

composer req webserver
composer req cli

Après l'exécution composer req cli, notez comment la
assets:install commande a été exécutée automatiquement.
Le bin/console fichier a également été ajouté à votre projet.

composer rem cli
pour supprimer le package cli
.. qui supprime également le bin/console binaire

composer req sec-checker : installer le contrôleur de sécurité SensioLabs;
composer req req-checker : pour installer les vérifications des exigences Symfony;
composer req log : installer MonologBundle et toutes ses dépendances;
composer req template : pour Twig;
composer req mailer : pour Swiftmailer;
composer req profiler : pour le profiler;

composer req debug-pack
composer req doctrine/doctrine-bundle
composer req doctrine/orm
composer raq sensio/generator-bundle

composer req symfony/validator
composer req symfony/form


#Cache:clear avec warmup
Before
./bin/console cache:clear (— -env=prod or dev)
Now
./bin/console cache:clear — -no-warmup (— -env=prod or dev)
./bin/console cache:warmup (— -env=prod or dev)

#Composant ClassLoader
si vous utilisez PHP 7 et la version 3.3 vous pouvez dores et déjà
tester en supprimant la ligne $kernel->loadClassCache(); dans votre
controller

#On en a fini avec SYMFONY_
SYMFONY_ ne sera plus interprété en version 4.x

il faut utiliser: %env()% (à partir de 3.2.x)
* app/config/parameters.yml
parameters:
    database_host: '%env(DATABASE_HOST)%'

env(DATABASE_HOST): localhost

#Insensibilité à la casse des identifients de services
services:
    MyService:
        class: AppBundle\RamdomClass

services:
    myService:
        class: AppBundle\AnotherClass

#Flash message
Before : app.session.started
Now : app.flashes

{% for label, messages in app.flashes %}
 {% for message in messages %}
   {{ message }}
 {% endfor %}
{% endfor %}

{% for messages in app.flashes(‘notice’) %}
  <div class=‘alert alert-notice’>
   {{ message }}
  </div>
{% endfor %}

{% for label, messages in app.flashes([’warning’,’error’]) %}
 {% for message in messages %}
   {{ message }}
 {% endfor %}
{% endfor %}

#Le username des utilisateurs in memory ne sont plus normalisés

Before
foo-bar@gmail.com -> foo_bar@gmail.com

Now
foo-bar@gmail.com -> foo-bar@gmail.com

#Symfony\Bundle\FrameworkBundle\Controller\Controller

. On encourage plus à utiliser le
Symfony\Bundle\FrameworkBundle\Controller\AbstractController

. fournit les mêmes helpers que le controller, mais ne donne plus accès au
container via la methode get()

.Encourager la déclaration des dépendances dans son controller

#Recherche dans le contenu d'un dump()
-dans la WebDebugToolbar
-dans le profiler
-dans une page

#Implementation de la psr-16
getMultiple($value)
setMultiple($value, $ttl = null)
deleteMultiple($value)
```
use Symfony\Component\Cache\Adapter\FilesystemAdapter;
$cache = new FilesystemAdapter();

// sauvegarde un élément en cache
$cache->set('stats.num_products', 4711);

//récupérer un élément du cache
if(!$cache->has('stats.num_products')){
  // ...item does not exits in the cache
} else {
  $total = $cache->get('stats.num_products');
}

//supprimer un élément du cache
$cache->delete('stats.num_products');
```

#Implementation de la psr-11
Symfony\Component\DependencyIngection\ContainerInterface
étend la Psr\Container\ContainerInterface
get() & has()
2 nouvelles Exceptions: ContainerExceptionInterface & NotFoundExceptionInterface

#WebserverBundle
.code déplacé
. $ bin/console server:start trouve un port disponible pour vous.

#FQCN comme id de service
-Before
```
services:
    app.manager.user:
        class: AppBundle\EventListener\UserManager
        tags: ['kernel.event_subscriber']

use AppBundle\EventListener\UserManager;

//...
public function editAction(){
  $this->get('app.manager.user')->save($user);
}
```
-Now
```
services:
    AppBundle\EventListener\UserManager:
        tags: ['kernel.event_subscriber']

use AppBundle\EventListener\UserManager;

//...
public function editAction(){
  $this->get('UserManager::class')->save($user);
}
```

#Nouveau data collector : cache dans le profiler

#Composant Dotenv
Charge, parse vos fichiers .env et popule vos variable d'environnement

#Topics sur github
framework | php | symfony | symfony-bundle | bundle | psr-3 | psr-11 | psr-6 | psr-16
http://symfony.com/blog/standardizing-the-github-topics-for-symfony-repositories

#Source Webographie
https://speakerdeck.com/saro0h/symfonylive-paris-quoi-de-neuf-depuis-1-an
