=======================
Using a custom buildout
=======================
 
First step, you will need to copy the ``buildout.cfg_tmpl`` into the
buildout root, and then select the profile you want to run::

 $ cp profiles/buildout.cfg.tmpl buildout.cfg
 $ vi buildout.cfg

Then you need to run::

 $ virtualenv env
 
This will create an env directory with a virtual environment. You should then
install the versions of zc.buildout and setuptools you need:

 $ env/bin/pip install -r requirements.txt

To create an instance, run::

 $ env/bin/buildout
 
This will download Plone's eggs and products for you, as well as other 
dependencies, create a new Zope 2 installation, and create a new Zope instance
configured with these products.

You can start your Zope instance by running::

 $ bin/instance start
 
or, to run in foreground mode::

 $ bin/instance fg
 
To run unit tests, you can use::

 $ bin/instance test -s my.package
 

Working with buildout.cfg
-------------------------

You can change any option in buildout.cfg and re-run bin/buildout to reflect
the changes. This may delete things inside the 'parts' directory, but should
keep your Data.fs and source files intact.

To save time, you can run buildout in "offline" (-o) and non-updating (-N)
mode, which will prevent it from downloading things and checking for new
versions online::

 $ bin/buildout -Nov

Extending buildout configs
--------------------------

This buildout makes use of the 'extends' functionality of buildout.  The
buildout.cfg contains only minimal information.  Here are what the rest of the
configs are for.

buildout.cfg.tmpl
  This is a template to be used for the buildout.cfg at the root of the
  site. See the file for more details.

base.cfg
  The base config contains all of the configuration for the basis of the site.
  Typical sections include zope2, instance, zeoserver and plone.  In this
  config we include all the eggs and products that will be used in all of the
  extended configs.

local.cfg
  The local config sets up our local development environment for us.  It
  includes all the debugging packages that are typically used during
  development.  It extends base.cfg and debug.cfg

debug.cfg
  The debug config contains all of our debugging products and packages. One
  package to make note of is PDBDebugMode.  It will open up a pdb prompt
  anytime there is an error.  This will cause the page to hang until you tell
  pdb to (c)ontinue.
  
  The debug config also contains a way to 'refresh' your product in
  plone.reload.  You can access it like this::
  
    http://<zope_host>:<zope_port>/@@reload
  
  And also a way of recording doctests::
  
    http://<zope_host>:<zope_port>/++resource++recorder/index.html
  
  Take a look at the config to see what other tools are available.

release.cfg
  The release config is the base config for doing releases.  It contains the
  specific versions of eggs that are needed to make the site run properly.  It
  also contains some configuration that is common for each release stage.

versions.cfg
  This contains the pinned versions of packages for use when release to production

testing.cfg
  The dev config merely sets up the proper port and ip-address for the dev
  site to run on.

staging.cfg
  The maint config merely sets up the proper port and ip-address for the maint
  site to run on.

prod.cfg
  The prod config is similar to the dev and maint configs in that it sets up
  the proper ip-address and port numbers.  But it can also be used to set up a
  Zope cluster, tune the number of threads being used, bump up zeo cache
  sizes, set up pound, squid, nginx, etc.  This will be the config used to run
  the site in production mode.

Depending on a new egg
----------------------

If you want to use a new egg that is in the Python Package Index, all you need
to do is to add it to the "eggs" option under the main ``[buildout]`` section::

 [buildout]
 ...
 eggs =
    my.package
    
If it's listed somewhere else than the Python Package Index, you can add a link
telling buildout where to find it in the 'find-links' option::

 [buildout]
 ...
 find-links =
    http://dist.plone.org
    http://download.zope.org/distribution/
    http://effbot.org/downloads
    http://some.host.com/packages
    

=============
Using Windows
=============

TODO: Rewrite this section
