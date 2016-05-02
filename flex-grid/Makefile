# aliases

## src

BUNDLE_JS = src/bundle.js
BUNDLE_CSS = src/bundle.css
MAIN_JS = src/scripts/main.js
MAIN_LESS = src/styles/main.less

SRC_FONT = src/fonts
SRC_IMG = src/img

# targets

## main targets

all: clean dist

deploy: all
	@rsync -av --rsh="sshpass -p PASSWORD ssh -l 'example.net'" dist/ example.net:/path/to/deploy

dist: $(BUNDLE_JS) $(BUNDLE_CSS) $(SRC_FONT) $(SRC_IMG)
	@mkdir -p $@
	@cp -r src/index.html $(BUNDLE_JS) $(BUNDLE_CSS) $(SRC_FONT) $(SRC_IMG) dist

watch: all
	@$(MAKE) -j watch-js watch-less

clean:
	@rm -rf $(BUNDLE_JS) $(BUNDLE_CSS) $(SRC_FONT) dist

.PHONY: all watch clean

## sub targets

watch-js: node_modules
	@node_modules/.bin/watchify $(MAIN_JS) -o $(BUNDLE_JS) -v -d

watch-less: node_modules
	@node_modules/.bin/watchf src/styles/**/*.less -c 'make -B $(BUNDLE_CSS)'

$(BUNDLE_JS): node_modules
	@node_modules/.bin/browserify $(MAIN_JS) -o $@

$(BUNDLE_CSS): node_modules
	@node_modules/.bin/lessc $(MAIN_LESS) --autoprefix $@

$(SRC_FONT): node_modules
	@mkdir -p $@
	@rsync -a node_modules/font-awesome/{css,fonts} $@

.PHONY: watch-less dist
