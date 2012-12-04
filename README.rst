VAGRANT
=======

.. contents::

Getting Started
---------------

Installing rbenv
~~~~~~~~~~~~~~~~
Rather than installing ruby via your distribution's package manager it is
recommended that you install ruby via ``rbenv``.  By using ``rbenv`` you ensure
all ruby gems and commands are run out of ``~/.rbenv`` which enables you to be
able to clear ruby completely off your workstation by simply deleting
``~/.rbenv``.

.. Note::
    * Since we're using rbenv we won't need to use sudo when installing gems.

    * Any time you install a new Ruby binary you should rerun the `rbenv
      rehash` command.


The only distro specific step below is when installing the dependencies, so
just modify the install line to fit your package manager and appropriate
package names.

.. code-block::

    # install dependencies (apt-get in ubuntu shown here)
    sudo apt-get install git virtualbox libreadline-dev libssl-dev

    # fetch rbenv
    git clone git://github.com/sstephenson/rbenv.git ~/.rbenv
    mkdir -p ~/.rbenv/plugins
    git clone git://github.com/sstephenson/ruby-build.git ~/.rbenv/plugins/ruby-build
    echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bashrc
    echo 'eval "$(rbenv init -)"' >> ~/.bashrc

.. Warning::
    You must restart your shell before continuing so ``rbenv`` is added to your ``$PATH``!


Installing Ruby
~~~~~~~~~~~~~~~~
At this point you should have the ``rbenv`` command in your ``$PATH``.  Now we
can move on to installing Ruby.

.. code-block::

    rbenv install 1.9.3-p327
    rbenv rehash
    rbenv global 1.9.3-p327

.. Tip::
    By typing ``rbenv install <tab><tab>`` you should be presented with a list
    of available versions.  Simply select the latest released version and
    continue.  Here we show installing version ``1.9.3-p327``

Installing Vagrant + Veewee
~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block::

    gem install vagrant veewee --no-ri --no-rdoc
    rbenv rehash


Building the Minimal CentOS Box
-------------------------------
Let's build the custom minimal CentOS 6 box based on the files in the
definitions directory.

.. code-block::

    vagrant basebox build 'centos-6.3-minimal'

.. Note::
    * The ``build`` command will handle retrieving (and caching) the required
      iso files to the ``iso`` directory the first time it is executed.

    * You may be hit with a warning during provisioning where you are asked
      whether to reinitialize the drive or not.  This is expected and you can
      freely reinitialize the drive without concern.

Once the machine has provisioned, rebooted, and run the cleanup script you
should be able to log in as ``root`` and shut the machine down.

Now, let's export the built image to a vagrant box.

.. code-block::

    mkdir -p exports
    cd exports
    vagrant basebox export 'centos-6.3-minimal'

.. Tip::
    From here you could place the exported vagrant box on an http accessible
    url and let your co-workers retrieve it for their own purposes.

Once we have a vagrant box we should go ahead and add it to our vagrant
inventory so we can start building machines.

.. code-block::

    vagrant box add 'centos-6.3-minimal' 'centos-6.3-minimal.box'


Running the Minimal CentOS Box
------------------------------
Let's instantiate a virtual machine based on our new minimal centos box.

.. code-block::

    mkdir -p machines/testvm
    cd machines/testvm
    vagrant init centos-6.3-minimal

.. Tip::
    You can set the virtual machine's hostname if you'd like by editing the
    ``Vagrantfile`` and adding the line ``config.vm.host_name = "testvm"``

**Starting the VM**

.. code-block::

    vagrant up

**Destroying the VM**

.. code-block::

    vagrant destroy

**Connecting to the VM**

.. code-block::

    vagrant ssh

**Display ssh configuration for the VM**

.. code-block::

    vagrant ssh-config