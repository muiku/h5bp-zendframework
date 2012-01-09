# HTML5 Boilerplate Zend Framework Integration

### Create a new project

    cd ~/workspace
    zf create project myproject

    cd h5bp-zendframework/build/
    ./createproject.sh public

    cd ../../
    cp -r public/ myproject/
    rm -rf public/

    cd myproject/
    zf enable layout
    mv public/layout.phtml application/layouts/scripts/layout.phtml

### Development

Just edit any file as you have done before:

- Put your styles into public/css/style.css
- Add some images under public/img/
- JavaScript goes into public/js/scripts.js
- etc. etc.

### Deployment (Apache 2)

Steps:

1) Run the H5BP build script

2) Change the site docroot to point into public/publish/

3) Change the production layouts folder in application.ini and
   remember to change APPLICATION_PATH one folder lower in
   public/publish/index.php

---

1) This is where the H5BP shines: a build script to optimize static files for the Web.

    cd public/build/
    ant # The H5BP build script will begin to run and compress your files.

After successful build we have got two new folders:

- public/publish/
- application/layouts/scripts/publish/

2) Change the docroot to point into public/publish/

    <VirtualHost *:80>
        # ...

        SetEnv APPLICATION_ENV production

    	DocumentRoot /home/me/workspace/myproject/public/publish
        DirectoryIndex index.php

        <Directory /home/me/workspace/myproject/public/publish>
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

    ; Published (H5BP build script generated) layouts
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

If you have more layouts in addition to ZF default layout (layout.phtml), edit public/build/project.xml to add those.

## License

### Major components:

* jQuery: MIT/GPL license
* Modernizr: MIT/BSD license
* Respond.js: MIT/GPL license
* Normalize.css: Public Domain

### Everything else:

The Unlicense (aka: public domain)
