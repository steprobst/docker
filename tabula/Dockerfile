FROM alpine:3.17

#Path for tabular 
ENV TPATH="/tabula"
#tabular version - stable not running
ENV TVER="1.2.1" 
#Java Version
ENV JVER="17"
#NGRMAS DATA for languages
ENV URL="https://github.com/tabulapdf/tabula/releases/download/v$TVER/tabula-jar-$TVER.zip"
ENV DATA_DIR = /.tabula
ENV DOCUMENTS_BASEPATH = /.tabula/pdfs


RUN apk update && \
    apk upgrade --no-cache && \
    apk add --no-cache openjdk$(echo $JVER)-jre curl unzip wget && \
    wget $URL && \
    unzip tabula-jar-$TVER.zip && \ 
    mkdir -p $DATA_DIR $DOCUMENTS_BASEPATH && \
    chown nobody $DATA_DIR $DOCUMENTS_BASEPATH && \
    rm tabula-jar-$TVER.zip  
    
WORKDIR $TPATH 

EXPOSE 8080

USER nobody 

CMD ["java", "-Dfile.encoding=utf-8", "-Xms256M", "-Xmx1024M", "-jar" ,"tabula.jar" ]
