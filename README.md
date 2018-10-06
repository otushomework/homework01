[![Build Status](https://travis-ci.org/otushomework/homework01.svg?branch=master)](https://travis-ci.org/otushomework/homework01)

- download GPG key from https://bintray.com/user/downloadSubjectPublicKey?username=bintray to ~/Downloads/bintray-public.key.asc
- sudo apt-key add ~/Downloads/bintray-public.key.asc
- add 'deb https://dl.bintray.com/kafeg/otus trusty main' to /etc/apt/sources.list
- sudo apt-get update
- sudo apt-get install helloworld
