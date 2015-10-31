XR.Mono.Cover (c) 2013 Ian Norton
==================================

About
------

XR.Mono.Cover is a simple code coverage tool specifically for mono. 
It will record which lines of your source are actually executed and 
log some basic statistics like call counts and the number of times 
a line is actually run.

![screenshot](https://raw.github.com/inorton/XR.Baboon/master/screenshots/baboon-001.png "Baboon Screenshot!")

It is built on top of Mono.Debugger.Soft and so requires the mono 
runtime rather than the .Net one.

Results are gathered by the covtool program and can be 
displayed/analysed by the cov-console or cov-gtk programs.

Coverage data generated by covtool ( via Mono.XR.Cover library ) is 
saved in a Sqlite database file for easy processing and quick 
generation.

covtool will run any mono/c# process you ask of it, such as 
nunit-console or even graphical apps. In time covtool may allow you 
to attach to existing processes after they have started.

XR.Mono.Cover will not check coverage of unmanaged code, perhaps 
one day if Mono.Debugger.Soft does, this will too.

I'm only aiming at linux but this should work well on OSX and Windows ( if you install mono ).

Like all good analysis tools, Baboon can test itself! See [here](https://raw.github.com/inorton/XR.Baboon/master/covtool/selftest.html) for some
results.

Baboon also includes a binary of nunit.framework.dll for it's tests.

Building
---------

Building baboon is fairly easy, you can use mdtool to build the solution file or load it in
monodevelop.

If you wish to make the bundled binary (not necessary but easier to deploy) do:

`$ bash make_bundle.sh`

Which should give you the 'covem' program (on linux anyway)

Installing
-----------
You can use the included Makefile to install baboon under Linux. 

Use `make install` to install without using make_bundle (see above).
Use `make installbundle` for installing the bundled version.

Running
--------

Be sure to deploy the `libsqlite3.so.0` C library wherever you run your app. If it is the same machine 
as the build was done on you needn't worry. Your 'test' program/assembly needs to have been built 
with debugging enabled (you need the mdb files) else baboon won't know how to inspect the 
running code.

First, we need to create a coverage config file to tell covem what classes/types you want to record
coverage data for. If you have a called called 'XR.HttpFileStream' and if your program is called 
'`myserver.exe`' then create a text file called '`myserver.exe.covcfg`' and add a line to it that
contains a regex to match your type name like so:

`^XR.HttpFileStream`

Save it, and then run your program as usual but put 'covem' (or the full path to the covem program 
or exe) atthe start of the line. Eg,

`$ /home/inb/tmp/covem myserver.exe --port 19000 --verbose`

And baboon will start the program and start recording coverage data.

Results
========

Looking at results is fairly easy, Once your program has ended ( or after every 2 minutes, or 
after sending it _SIGUSR2_ ) you will find a new file called '`myserver.exe.covdb`' simply launch
the cov-gtk app and load this file using the open button and you'll see coverage data as above.
