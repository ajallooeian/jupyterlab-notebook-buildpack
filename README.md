# Jupyter Notebook Buildpack
The `jupyterlab-notebook-buildpack` is a [Cloud Foundry][] buildpack for exposing [Jupyter Notebook][] files.

## Usage
To use this buildpack specify the URI of the repository when pushing a Jupyter Notebook file (or directory of files) to Cloud Foundry.

    cf push -b https://github.com/csylvester-pivotal/jupyterlab-notebook-buildpack <app-name>

## Conda Version
Create a file named `conda_runtime.txt` containing the Miniconda version you wish to use as conda-runtime. Otherwise `Miniconda-latest-Linux-x86_64.sh` (python 2.x) will be used.

Find different runtimes here: http://repo.continuum.io/miniconda/

Use `Miniconda3-3.7.3-Linux-x86_64.sh` for python 3.x support.

## Notebook Dependencies
The buildpack supports dependencies declaration using a `requirements.txt` file located in the root of the directory being pushed to Cloud Foundry.

## Notebook Config
Additional notebook configuration can be specified using a file named `additional_notebook_config.py` in the root of the app directory.

In particular you can password protect your notebook server by creating a SHA1 hash of your password and adding this to the config file as follows:

    # Password to use for web authentication
    c = get_config()
    # Format for line below is => 'algorithm:SALT:hash of PW+SALT'
    c.NotebookApp.password = u'sha1:85607904acb0:bd228c0989e8df2018d0777428b1bb974ba7d2c0'

Here are a couple of ways you can generate the correct password hash:
* Use the functionality available in Jupyter as described [here](https://jupyter-notebook.readthedocs.io/en/stable/public_server.html#preparing-a-hashed-password)
* From the Linux command line using `openssl` and `sha1sum`:
```
    $ SALT=$(openssl rand -base64 12)
    $ PW='your password'
    $ echo -n "${PW}${SALT}" | sha1sum | cut -f1 -d' '
```
Just remember you need to include the SALT value in the `c.NotebookApp.password` parameter if you want to shorten this to one line.

See [Jupyter documentation](https://jupyter-notebook.readthedocs.io/en/stable/public_server.html) for further instructions and more configuration options for the server.

## License
This buildpack is released under version 2.0 of the [Apache License](http://www.apache.org/licenses/LICENSE-2.0).

[Cloud Foundry]: http://www.cloudfoundry.com
[Jupyter Notebook]: https://jupyter-notebook.readthedocs.io
