JAR=./target/Enlight-0.2.jar
CUSTOM_LUCENE_VERSION=10.2.2-SNAPSHOT
PORT=8647

.PHONY: clean
clean:
	mvn clean

.PHONY: test
test:
	mvn test

.PHONY: build
build:
$(JAR):
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

~/.m2/repository/org/apache/lucene/lucene-queryparser/$(CUSTOM_LUCENE_VERSION)/lucene-queryparser-$(CUSTOM_LUCENE_VERSION).jar: lib/lucene-queryparser-$(CUSTOM_LUCENE_VERSION).jar
	mvn install:install-file \
	    -Dfile=$< \
	    -DgroupId=org.apache.lucene \
	    -DartifactId=lucene-queryparser \
	    -Dversion=$(CUSTOM_LUCENE_VERSION) \
	    -Dpackaging=jar \
	    -DgeneratePom=true

~/.m2/repository/org/apache/lucene/lucene-queries/$(CUSTOM_LUCENE_VERSION)/lucene-queries-$(CUSTOM_LUCENE_VERSION).jar: lib/lucene-queries-$(CUSTOM_LUCENE_VERSION).jar
	mvn install:install-file \
	    -Dfile=$< \
	    -DgroupId=org.apache.lucene \
	    -DartifactId=lucene-queries \
	    -Dversion=$(CUSTOM_LUCENE_VERSION) \
	    -Dpackaging=jar \
	    -DgeneratePom=true

.PHONY: install-custom-libs
install-custom-libs: ~/.m2/repository/org/apache/lucene/lucene-queryparser/$(CUSTOM_LUCENE_VERSION)/lucene-queryparser-$(CUSTOM_LUCENE_VERSION).jar ~/.m2/repository/org/apache/lucene/lucene-queries/$(CUSTOM_LUCENE_VERSION)/lucene-queries-$(CUSTOM_LUCENE_VERSION).jar
