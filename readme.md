# HTML5 Boilerplate Zend Framework Integration

[Compare to the original H5BP](https://github.com/muiku/h5bp-zendframework/compare/master...zfint)

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
    mv public/layout.phtml public/404.phtml application/layouts/scripts/

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
   remember to check that APPLICATION_PATH is correctly defined in
   public/publish/index.php

4) Update ErrorController to change 404 layout in production

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

    ; Ensure that view encoding is UTF-8 and that view helpers use HTML5
    resources.view.encoding = "UTF-8"
    resources.view.doctype = "HTML5"

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

4)

    <?php

class ErrorController extends Zend_Controller_Action
{

    public function errorAction()
    {
        $errors = $this->_getParam('error_handler');

        if (!$errors || !$errors instanceof ArrayObject) {
            $this->view->message = 'You have reached the error page';
            return;
        }

        switch ($errors->type) {
            case Zend_Controller_Plugin_ErrorHandler::EXCEPTION_NO_ROUTE:
            case Zend_Controller_Plugin_ErrorHandler::EXCEPTION_NO_CONTROLLER:
            case Zend_Controller_Plugin_ErrorHandler::EXCEPTION_NO_ACTION:
                // 404 error -- controller or action not found
                $this->getResponse()->setHttpResponseCode(404);
                $priority = Zend_Log::NOTICE;
                $this->view->message = 'Page not found';

                // Change layout to show user and crowler friendly 404 page
                if (APPLICATION_ENV === 'production') {
                    $this->_helper->layout->setLayout('404');
                }
                break;

    ...

## License

### Major components:

* jQuery: MIT/GPL license
* Modernizr: MIT/BSD license
* Respond.js: MIT/GPL license
* Normalize.css: Public Domain

### Everything else:

The Unlicense (aka: public domain)
