# HTML5 Boilerplate Zend Framework Integration

### Create a new project

    cd ~/workspace
    zf create project myproject

    cd html5-boilerplate/build/
    ./createproject.sh public

    cd ../../
    cp -r public/ myproject/public/
    rm -rf public

    cd myproject
    zf enable layout
    mv public/layout.phtml application/layouts/scripts/

### Development

Just edit files as you have done before. Put your styles into css/style.css, add some images under img/, javascript goes into /js/scripts.js... etc. etc.

### Deployment

Steps:

1) Run h5bp build script
2) Change the site docroot to point into public/publish/
3) Change production layouts folder in application.ini

---

1) This is where h5bp shines: a build script to optimize static files for the Web.

    cd public/build/
    ant # The H5BP build script will begin to run and compress your files.

After successfull build we have got a bunch of new folders:

- public/publish/
- application/layouts/scripts/publish

2) Change the docroot to point into public/publish/

    <VirtualHost *:80>
        # ...

        SetEnv APPLICATION_ENV production

    	DocumentRoot /home/me/sites/www/myproject/public/publish
    	<Directory /home/me/sites/www/myproject/public/publish>
	    	AllowOverride All
		    Order allow,deny
    		Allow from all
	    </Directory>

        # ...
    </VirtualHost>

3) Edit application.ini:

    [production]
    phpSettings.display_startup_errors = 0
    phpSettings.display_errors = 0
    includePaths.library = APPLICATION_PATH "/../library"
    bootstrap.path = APPLICATION_PATH "/Bootstrap.php"
    bootstrap.class = "Bootstrap"
    appnamespace = "Application"
    resources.frontController.controllerDirectory = APPLICATION_PATH "/controllers"
    resources.frontController.params.displayExceptions = 0
    resources.layout.layoutPath = APPLICATION_PATH "/layouts/scripts/publish"

    [staging : production]

    [testing : production]
    phpSettings.display_startup_errors = 1
    phpSettings.display_errors = 1
    resources.layout.layoutPath = APPLICATION_PATH "/layouts/scripts"

    [development : production]
    phpSettings.display_startup_errors = 1
    phpSettings.display_errors = 1
    resources.frontController.params.displayExceptions = 1
    resources.layout.layoutPath = APPLICATION_PATH "/layouts/scripts"

## License

### Major components:

* jQuery: MIT/GPL license
* Modernizr: MIT/BSD license
* Respond.js: MIT/GPL license
* Normalize.css: Public Domain

### Everything else:

The Unlicense (aka: public domain)
