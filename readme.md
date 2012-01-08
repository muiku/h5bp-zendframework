# HTML5 Boilerplate Zend Framework Integration

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
    cd public/build/
    ant

## License

### Major components:

* jQuery: MIT/GPL license
* Modernizr: MIT/BSD license
* Respond.js: MIT/GPL license
* Normalize.css: Public Domain

### Everything else:

The Unlicense (aka: public domain)
