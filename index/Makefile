all: help
SHELL=/bin/bash
.SECONDARY:
.DELETE_ON_ERROR:

GREEN=\033[1;32m
RED=\033[1;31m
BLUE=\033[1;34m
RESET=\033[0m

JAR=./target/Enlight-0.2.jar
CUSTOM_QUERYPARSER=~/.m2/repository/org/apache/lucene/lucene-queryparser/$(CUSTOM_LUCENE_VERSION)/lucene-queryparser-$(CUSTOM_LUCENE_VERSION).jar
CUSTOM_QUERIES=~/.m2/repository/org/apache/lucene/lucene-queries/$(CUSTOM_LUCENE_VERSION)/lucene-queries-$(CUSTOM_LUCENE_VERSION).jar
CUSTOM_JARS=$(CUSTOM_QUERYPARSER) $(CUSTOM_QUERIES)
CUSTOM_LUCENE_VERSION=10.2.2-SNAPSHOT
PORT=8647

$(CUSTOM_QUERYPARSER): lib/lucene-queryparser-$(CUSTOM_LUCENE_VERSION).jar
	mvn install:install-file \
	    -Dfile=$< \
	    -DgroupId=org.apache.lucene \
	    -DartifactId=lucene-queryparser \
	    -Dversion=$(CUSTOM_LUCENE_VERSION) \
	    -Dpackaging=jar \
	    -DgeneratePom=true

$(CUSTOM_QUERIES): lib/lucene-queries-$(CUSTOM_LUCENE_VERSION).jar
	mvn install:install-file \
	    -Dfile=$< \
	    -DgroupId=org.apache.lucene \
	    -DartifactId=lucene-queries \
	    -Dversion=$(CUSTOM_LUCENE_VERSION) \
	    -Dpackaging=jar \
	    -DgeneratePom=true

.PHONY: clean
clean:
	mvn clean

.PHONY: test
test: $(CUSTOM_JARS)
	mvn test

.PHONY: build
build:
$(JAR): $(CUSTOM_JARS)
	mvn package

.PHONY: run
run: $(JAR)
	java -jar $(JAR) -path ./data -port $(PORT) > enlight.log &

.PHONY: ingest
ingest:
	curl -X PUT "localhost:$(PORT)/ingest" -F "file=@index_test.json"

.PHONY: log
log:
	tail -f enlight.log

.PHONY: install-custom-libs
install-custom-libs: $(CUSTOM_JARS)

.PHONY: help
help:
	@echo "available \`make' targets:"
	@echo -e "  $(BLUE)install-custom-libs$(RESET)  - install the jars in 'lib/' in your local maven repository"
	@echo -e "  $(BLUE)test$(RESET)                 - run the tests"
	@echo -e "  $(BLUE)build$(RESET)                - build the enlight app"
	@echo -e "  $(BLUE)run$(RESET)                  - run the enlight app"
	@echo -e "  $(BLUE)ingest$(RESET)               - fill the enlight app"
	@echo -e "  $(BLUE)log$(RESET)                  - tail the enlight app logs"
	@echo -e "  $(BLUE)clean$(RESET)                - clean up build artefacts"
