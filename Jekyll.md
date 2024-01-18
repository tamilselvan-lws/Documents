## Prerequisites

- **Ruby:** Jekyll is built using Ruby, so you need to have Ruby installed. You can check if Ruby is already installed by running ruby -v in your terminal.

- **RubyGems:** This comes with Ruby by default, but you can ensure it's available by running gem -v.

## Install Jekyll

Install Ruby and other prerequisites:

```
sudo apt-get install ruby-full build-essential zlib1g-dev
```

Avoid installing RubyGems packages (called gems) as the root user. Instead, set up a gem installation directory for your user account. The following commands will add environment variables to your ~/.bashrc file to configure the gem installation path:

```
echo '# Install Ruby Gems to ~/gems' >> ~/.bashrc
echo 'export GEM_HOME="$HOME/gems"' >> ~/.bashrc
echo 'export PATH="$HOME/gems/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
```

Finally, install Jekyll and Bundler:

```
gem install jekyll bundler
```

That’s it! You’re ready to start using Jekyll.

## Creating a repository for your site

