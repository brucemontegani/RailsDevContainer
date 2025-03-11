ARG RUBY_VERSION=3.2
FROM ruby:$RUBY_VERSION

# Set environment variables
ENV DEBIAN_FRONTEND=noninteractive

# Define arguments
ARG USERNAME=vscode
ARG DATABASE=sqlite

# Install system dependencies
RUN apt-get update -qq && apt-get install --no-install-recommends -y \
    build-essential \
    imagemagick \
    git \
    zsh \
    sudo \
    curl \
    wget \
    ca-certificates \
    fonts-powerline

# Conditional database installation
RUN if [ "$DATABASE" = "sqlite" ]; then \
wget https://www.sqlite.org/2025/sqlite-autoconf-3490100.tar.gz && \
      tar xvfz sqlite-autoconf-3490100.tar.gz && \
      cd sqlite-autoconf-3490100 && \
      ./configure && \
      make -j$(nproc) && \
      sudo make install && \
      cd .. && rm -rf sqlite-autoconf-3490100 sqlite-autoconf-3490100.tar.gz; \
    elif [ "$DATABASE" = "postgres" ]; then \
    apt-get install --no-install-recommends -y postgresql-client libpq-dev; \
    elif [ "$DATABASE" = "mysql" ]; then \
    apt-get install --no-install-recommends -y mysql-client libmysqlclient-dev; \
    fi \
    && rm -rf /var/lib/apt/lists/*

# Set up user
RUN mkdir -p /etc/sudoers.d && \
    useradd -m -s /usr/bin/zsh $USERNAME && \
    mkdir -p /home/$USERNAME && \
    chown $USERNAME:$USERNAME /home/$USERNAME && \
    echo "$USERNAME ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers.d/$USERNAME

# Install Oh-My-Zsh
USER $USERNAME
RUN sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)" || true

# Install Pure prompt for Oh-My-Zsh
RUN mkdir -p "/home/$USERNAME/.zsh"
RUN git clone https://github.com/sindresorhus/pure.git "/home/$USERNAME/.zsh/pure"
RUN echo 'fpath+=/home/$USERNAME/.zsh/pure' >> "/home/$USERNAME/.zshrc"
RUN echo 'autoload -U promptinit; promptinit' >> "/home/$USERNAME/.zshrc"
RUN echo 'prompt pure' >> "/home/$USERNAME/.zshrc"

# Set working directory
WORKDIR /workspace

# Install Rails
ARG RAILS_VERSION
# Install Rails based on the version specified but if not specified, install the latest version.
RUN if [ -z "$RAILS_VERSION" ] ; then gem install rails ; else gem install rails -v $RAILS_VERSION ; fi

# Set default shell to Zsh
CMD ["zsh"]
