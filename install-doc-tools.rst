.. _`install-doc-tools`:

This section of the guide cover the installation process and basic usage of the tools
needed in order to make changes to the documentation.

You are going to need the following tools:

- `Git <http://en.wikipedia.org/wiki/Git_(software)>`_
- `Python <https://www.python.org/>`_ and `pip <https://en.wikipedia.org/wiki/Pip_(package_manager)>`_ (recent versions of Python come bundled with pip)
- `Sphinx <http://sphinx-doc.org/index.html>`_

Install Git
-----------

To install Git on your machine refer to `this <https://git-scm.com/book/en/v2/Getting-Started-Installing-Git>`_
guide.

For a brief introduction to Git usage refer to `this <https://git-scm.com/book/en/v2/Getting-Started-Git-Basics>`_ guide

Install Python
--------------

Python installation is pretty straight forward, refer to the `official download page <https://www.python.org/downloads/>`_
and follow the instruction for your Operating System. As stated above from Python 2.7.9 onward pip
comes bundled with Python.

Open the terminal on your machine and type  `python`, the output should resemble the following:::

    > python
    Python 2.7.6 (default, Jun 22 2015, 17:58:13)
    [GCC 4.8.2] on linux2
    Type "help", "copyright", "credits" or "license" for more information.
    >>>

Type `exit()` to exit the Python interpreter

If you get and error back make sure the Python executable is in your `PATH`.

If you are on Windows follow `theese instructions <https://docs.python.org/2/using/windows.html#excursus-setting-environment-variables>`_

If you are on Linux locate the Python binary and add it to your PATH, edit your `~/.bashrc`
and add the following at the end of the file:::

    export PATH=$PATH:/path/to/the/python-binary

restart your terminal to make the change effective

Now check that you can invoke pip typing `pip --version`. The output should resemble the following:::

    pip 1.5.4 from /usr/lib/python2.7/dist-packages (python 2.7)

If you get and error back make sure the pip executable is in your `PATH`.

Install Sphinx
--------------

To install Sphinx on your machine, type the following in your terminal:::

    pip install Sphinx

.. note:: If you are on Linux you may need to prefix the command with `sudo`

Now test your installation:::

    > sphinx-build --version
    Sphinx (sphinx-build) 1.3.1

For a more detailed installation guide refer to `this document <http://docs.geoserver.org/latest/en/docguide/install.html>`_

Check out documentation source
------------------------------

Since write acecss to the main documentation repository is restricted you may want
to work on your personal `fork <https://help.github.com/articles/fork-a-repo/>`_.
Login on Github with your account, navigate to the `documentation reposiroty <https://github.com/geosolutions-it/doc-geonode>`_
and click on `fork`. You will be redirected to your own fork of the documentation.

Make sure you already `configured your Git username and email address <https://help.github.com/articles/set-up-git/#setting-up-git>`_

Now clone your repository locally:::

    git clone https://github.com/your-user/doc-geonode.git

where `your-user` is your username on GitHub.

Now make the changes you want to the documentation.

Build the documentation
-----------------------

To build the documentation locally on your machine, open the terminal and move to
the project root directory, then run the following:::

    make html

The html version of the documentation will be build under the `build` subfolder.
Use your favorite web browser to open the index file called `index.html`.

When you make changes to the documentation re-build it periodically to make sure
the end result matches what you expect.

Edit the documentation
----------------------

The documentation is kept in reStructuredText format.
For a quick reference refer to `this <http://docs.geoserver.org/latest/en/docguide/sphinx.html>`_
document. For a more in-depth document refer to the `official documentation
<http://docutils.sourceforge.net/docs/ref/rst/restructuredtext.html>`_.


Submit your changes to the main repository
------------------------------------------

You are done making changes to the documentation and you are ready to submit your
changes.
If you are new to Git make sure you grasped the basics of it before moving on. There
are many sources of information online, you can Google for it or read `this <https://git-scm.com/book/en/v2/Getting-Started-Git-Basics>`_
introduction.

Run `git status` and `git diff` to review the changes you made and re-build the documentation
locally as explained above.

Add the files with the changes you want to submit to the staging area::

    git add path/to/file

And commit the changes::

    git commit

Then push them to your personal GitHub repository::

    git push origin master

Open your favorite browser and navigate to your GitHub repository. You will be
able to see your latest commit along with a message stating that your branch is
ahead of the main repository. Click on the 'pull request' button to make a pull
request against the official documentation repository.

Create a pull request with a brief description of what you did. The pull request
will be reviewed and eventually merged into the official documentation repository.
