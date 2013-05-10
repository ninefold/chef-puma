Puma Cookbook
=============

Chef cookbook for the [puma](http://puma.io) server.

The defaults assume you are deploying with capistrano and will write all configuration and logs to the shared/puma directory... However the configuration should be flexible enough to support any deployment setup.

The cookbook will also setup scripts to support restarts and phased restarts.

By default puma_config will enable monit monitoring and log rotation via logrotate.


Requirements
------------

monit, logrotate


Usage
-----
Basic puma configuration using defaults based off the application name:

    puma_config "app"
  
Custom config overriding app settings. In this example the configuration files and helper scripts will be placed in /srv/app/shared/puma. 

For example - in the following example to do a 'phased-restart' (zero downtime deploy) after deploying your code you would run the /srv/app/shared/puma/puma_phased_restart.sh script.

    puma_config "app" do
      directory "/srv/app"
      environment 'staging'
      monit false
      logrotate false
      thread_min 0
      thread_max 16
      workers 2
    end

 
Common Settings
-----

directory: Working directory of your app. This is where config.ru is.

puma_directory: directory where sockets, state and logs will be stored. Defaults to <directory>/shared/puma

environment: (default = production)

bind: defults to unix socket (at unix:///srv/app/shared/puma/app.sock) can speficy TCP socket instead such as "tcp://0.0.0.0:9292"

exec_prefix: default bundle exec 

thread_min: min number of threads in puma threadpool 

thread_max: max number of threads in puma threadpool 

workers: number of worker processes defaults to 0, must be greater than 0 for phased restarts

monit: (true) set to false to disable monit functionality

logrotate: (true) set to false to disable logrotate functionality

Attributes
==========

See the `attributes/default.rb` for default values. 


* `node['puma']['version']` - Version of puma to install
  
* `node['puma']['bundler_version']` - Version to bundler to install


Issues
------
Find a bug? Want a feature? Submit an [issue here](http://github.com/yourabi/chef-puma/issues). Patches welcome!


Contributing
------------

1. Fork the repository on Github

2. Create a named feature branch (like `add_component_x`)
 
3. Write you change
 
4. Write tests for your change (if applicable)
 
5. Run the tests, ensuring they all pass

6. Submit a Pull Request using Github


License and Authors
-------------------

License: Apache

Authors: Yousef Ourabi

About: This was originally developed for use at Burstorm 

    http://www.burstorm.com

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
