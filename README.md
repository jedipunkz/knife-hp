Knife HP
========

This is the official Opscode Knife plugin for HP Cloud. This plugin gives knife the ability to create, bootstrap and manage instances in the HP Cloud.

# Installation #

Be sure you are running the latest version Chef. Versions earlier than 0.10.0 don't support plugins:

    $ gem install chef

This plugin currently depends on HP's fork of Fog. To install it, run:

    $

This plugin is distributed as a Ruby Gem, but is not available on Rubygems.org because of the missing Fog dependencies. To install it, run:

    $

Depending on your system's configuration, you may need to run this command with root privileges.

# Configuration #

In order to communicate with HP Compute Cloud's API you will need to tell Knife XXX. The easiest way to accomplish this is to create these entries in your `knife.rb` file:

    knife[:hp_account_id] = "Your HP account ID"
    knife[:hp_secret_key] = "Your HP secret key"

If your knife.rb file will be checked into a SCM system (ie readable by others) you may want to read the values from environment variables:

    knife[:hp_account_id] = "#{ENV['HP_ACCOUNT']}"
    knife[:hp_secret_key] = "#{ENV['HP_SECRET]}"

You also have the option of passing your HP API Username/Password into the individual knife subcommands using the `-A` (or `--hp-account`) `-K` (or `--hp-secret`) command options

    knife hp server create -A 'MyUsername' -K 'MyPassword' -f 1 -I 13 -S matt -r 'role[webserver]'

Additionally the following options may be set in your `knife.rb`:

* flavor
* image
* hp_ssh_key_id
* template_file

# Subcommands #

This plugin provides the following Knife subcommands. Specific command options can be found by invoking the subcommand with a `--help` flag

knife hp server create
----------------------

Provisions a new server in the HP Compute Cloud and then perform a Chef bootstrap (using the SSH protocol). The goal of the bootstrap is to get Chef installed on the target system so it can run Chef Client with a Chef Server. The main assumption is a baseline OS installation exists (provided by the provisioning). It is primarily intended for Chef Client systems that talk to a Chef Server. By default the server is bootstrapped using the [ubuntu10.04-gems](https://github.com/opscode/chef/blob/master/chef/lib/chef/knife/bootstrap/ubuntu10.04-gems.erb) template. This can be overridden using the `-d` or `--template-file` command options.

knife hp server delete
----------------------

Deletes an existing server in the currently configured HP Compute Cloud account. <b>PLEASE NOTE</b> - this does not delete the associated node and client objects from the Chef Server.

knife hp server list
--------------------

Outputs a list of all servers in the currently configured HP Compute Cloud account. <b>PLEASE NOTE</b> - this shows all instances associated with the account, some of which may not be currently managed by the Chef Server.

knife hp flavor list
--------------------

Outputs a list of all available flavors (available hardware configuration for a server) available to the currently configured HP Compute Cloud account. Each flavor has a unique combination of virtual cores, disk space and memory capacity. This data can be useful when choosing a flavor id to pass to the `knife hp server create` subcommand.

knife hp image list
-------------------

Outputs a list of all available images available to the currently configured HP Compute Cloud account. An image is a collection of files used to create or rebuild a server. This data can be useful when choosing an image id to pass to the `knife hp server create` subcommand.

# TODO #

This is a list of features currently lacking and (eventually) under development:

* need an ohai plugin to populate `cloud` and `hp` attributes
* take either the flavor ID or the flavor name
* take either the image ID or the image name

# License #

Author:: Matt Ray (<matt@opscode.com>)

Copyright:: Copyright (c) 2012 Opscode, Inc.

License:: Apache License, Version 2.0

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.