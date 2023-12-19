FROM alpine:3.17

#Path for Languagetool
ENV LPATH="/languagetool"
#LangagueTool Version - stable not running
ENV LVER="6.1" 
#Java Version
ENV JVER="17"
#NGRMAS DATA for languages
ENV NGRAMS="de-20150819 en-20150817 es-20150915 fr-20150913 nl-20181229"
#HTTP URL for Downloads
ENV NGRAMSURL="https://languagetool.org/download"

RUN apk update && \
    apk upgrade --no-cache && \
    apk add --no-cache unzip wget && \
    mkdir -p $LPATH/ngrams && \
    cd $LPATH/ngrams && \
    for var in $NGRAMS; do wget $NGRAMSURL/ngram-data/ngrams-$var.zip; unzip ngrams-$var.zip; rm ngrams-$var.zip; done
