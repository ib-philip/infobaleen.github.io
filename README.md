## Infobaleen Documentation

The documentation uses [MkDocs](https://www.mkdocs.org/), which is a fast and simple static site generator geared towards building project documentation. Documentation source files are written in Markdown, and configured with a single YAML configuration file.


## How to create a static website for the docs?

We highly recommend to create a python virtual environment.

    - $ virtualenv venv -p python3

Activate the virtual environment:

    - $ source venv/bin/activate

Install the requirements:

    - $ pip install -r requirements.txt



### Install MKDocs

To install MkDocs, run the following command from the command line (requires python):

```pip install mkdocs```


### Run the documentation locally (using the terminal)

1. Download the github pages repo:
    * `git clone https://github.com/infobaleen/infobaleen.github.io.git`

2. Check the generated version
    * Run `mkodcs serve`. This will serve the local version of the documentation at `http://127.0.0.1:8000`
    * In the command window you might se `WARNING` messages. This is often for files that are not included in the documentation. Add these in the menu in `mkdocs.yml`


### Procudure for updating the online documentation 

1. Download the customer-succes repo:
    * First time its done: Run `git clone https://github.com/infobaleen/customer-success.git` 
    * Subsequent runs: Go to the local folder `customer-success` and run `git pull`

2. Copy the folder `customer-success/Documentation/Platform` and replace the old one in `infobaleen.github.io/docs/`

3. Push changes to Github
    * If you did any changes in step 2, push these to Github by:
    	* `git add -A`
    	* `git commit -m "Your comment: Added latest changes"`
    	* `git push`

4. Push changes and update the online Github page (using the terminal)
     * `mkdocs build`, and after that 
     * `mkdocs gh-deploy` 

5. Shortly afterwards you should be able to see the changes at [https://infobaleen.github.io/](https://infobaleen.github.io/)


Note