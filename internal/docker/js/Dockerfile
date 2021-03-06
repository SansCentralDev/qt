FROM ubuntu:16.04
LABEL maintainer therecipe

ENV USER user
ENV HOME /home/$USER
ENV GOPATH $HOME/work
ENV PATH /usr/local/go/bin:$PATH
ENV QT_DIR /opt/Qt
ENV QT_DOCKER true
ENV QT_QMAKE_DIR /opt/qtbase/bin

COPY --from=therecipe/qt:linux /usr/local/go /usr/local/go
COPY --from=therecipe/qt:linux $GOPATH/bin $GOPATH/bin
COPY --from=therecipe/qt:linux $GOPATH/src/github.com/therecipe/qt $GOPATH/src/github.com/therecipe/qt
COPY --from=therecipe/qt:linux /opt/Qt/5.11.1/gcc_64/include /opt/Qt/5.11.1/gcc_64/include
COPY --from=therecipe/qt:linux /opt/Qt/Docs /opt/Qt/Docs
COPY --from=therecipe/qt:js_base_4 $HOME/emsdk $HOME/emsdk
COPY --from=therecipe/qt:js_base_4 $HOME/.emscripten $HOME/.emscripten
COPY --from=therecipe/qt:js_base_4 /opt /opt

RUN apt-get -qq update && apt-get --no-install-recommends -qq -y install git && apt-get -qq clean
RUN apt-get -qq update && apt-get --no-install-recommends -qq -y install python2.7 nodejs cmake default-jre && apt-get -qq clean
RUN ln -s /usr/bin/python2.7 /usr/bin/python

RUN go get github.com/gopherjs/gopherjs

RUN $GOPATH/bin/qtsetup prep
RUN $GOPATH/bin/qtsetup check js
RUN $GOPATH/bin/qtsetup generate js
RUN $GOPATH/bin/qtsetup install js
RUN cd $GOPATH/src/github.com/therecipe/qt/internal/examples/widgets/line_edits && $GOPATH/bin/qtdeploy build js && rm -rf ./deploy
