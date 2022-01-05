FROM rbenv:latest

## Change here to set personal information ##
ARG DEBIAN_FRONTEND=noninteractive \ 
    DEFAULT_RUBY_VERSION=2.6.3 \
    SOURCE_DIR=src \
    GIT_URL=https://github.com/xvzc \
    REPO_NAME=xvzc.github.io.git

ENV USER_NAME jekyll
ENV HOME /home/$USER_NAME
## end ##
RUN adduser --gecos '' --disabled-password $USER_NAME;

# change user group
RUN usermod -g staff $USER_NAME; \
    usermod -G sudo $USER_NAME 

RUN mkdir -p $HOME/$SOURCE_DIR
WORKDIR $HOME/$SOURCE_DIR

# install ruby
RUN rbenv install $DEFAULT_RUBY_VERSION; \
    rbenv global $DEFAULT_RUBY_VERSION; \
    gem install bundler;

# oh my zsh
RUN sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"; \
    git clone https://github.com/zsh-users/zsh-syntax-highlighting.git \ 
	      ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting;

RUN mkdir $HOME/$SOURCE_DIR/$REPO_NAME; \ 
    git clone $GIT_URL/$REPO_NAME $HOME/$SOURCE_DIR/$REPO_NAME

# set path variable
ENV GEM_HOME $HOME/.gem 
ENV GEM_PATH $HOME/.gem 
ENV PATH $GEM_HOME:$PATH

# export ENV in .zshrc file to set environment variables for SSH connections
# set locales
RUN echo "export RBENV_ROOT=$(echo $RBENV_ROOT)" >> ~/.zshrc; \
    echo "export GEM_HOME=$(echo $GEM_HOME)" >> ~/.zshrc; \
    echo "export GEM_PATH=$(echo $GEM_PATH)" >> ~/.zshrc; \
    echo "export PATH=$(echo $PATH)" >> ~/.zshrc; \
    echo 'eval "$(rbenv init -)" #changed this to below because of slow startup' >> ~/.zshrc; 

# this line prevents the zsh compinit error which adds the following line to the top of .zshrc
RUN sed -i '1s/^/ZSH_DISABLE_COMPFIX="true"\n/' ~/.zshrc; \ 
    sed -i 's/robbyrussell/pmcgee/g' ~/.zshrc; \ 
    sed -i 's/plugins=(git)/plugins=(git zsh-syntax-highlighting)/g' ~/.zshrc; \ 
    echo 'set -o vi' >> ~/.zshrc;


# install app dependencies
RUN (cd $HOME/$SOURCE_DIR/$REPO_NAME && bundle install)

# change owner of $HOME 
RUN chown -R $USER_NAME:staff $HOME; \ 
    chsh -s $(which zsh) $USER_NAME;

RUN git config --global init.defaultBranch main

# run zsh with $USER_NAME to avoid .zompdump being created as root previlage
ENTRYPOINT service ssh restart && (su $USER_NAME; zsh)

EXPOSE 4000
EXPOSE 22
