FROM ngram:0.1 

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

RUN wget $NGRAMSURL/LanguageTool-$LVER.zip && \
    unzip LanguageTool-$LVER.zip && \ 
    rm LanguageTool-$LVER.zip && \ 
    mv /LanguageTool-$LVER/* $LPATH/ && \
    rm -rf /LanguageTool-$LVER 
    
WORKDIR $LPATH 
EXPOSE 8081

USER nobody 

CMD ["java", "-cp", "languagetool-server.jar", "org.languagetool.server.HTTPServer", "--port", "8081", "--public", "--allow-origin", "'*'", "--languageModel", "ngrams" ]
