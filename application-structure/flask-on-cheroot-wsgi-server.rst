Deploying Flask on Cheroot WSGI server
======================================

Running a Flask application on top of Cheroot_ is a piece of cake.

Here is the Flask hello world app for reference:

.. code-block:: python

    from flask import Flask
    app = Flask(__name__)

    @app.route("/")
    def hello():
        return "Hello World!"

    if __name__ == "__main__":
        app.run()

The following snippet let you run the Flask app on top of the WSGI server shipped with Cheroot.

.. code-block:: python

    from cheroot.wsgi import PathInfoDispatcher
    from cheroot.wsgi import Server
    from hello import app

    d = PathInfoDispatcher({'/': app})
    server = Server(('0.0.0.0', 8080), d)

    if __name__ == '__main__':
        try:
            server.start()
        except KeyboardInterrupt:
            server.stop()

You can access the server running on 0.0.0.0:8080. PathInfoDispatcher take as argument a dictionary mapping a path to an application object, so you can easily deploy multiple (Flask) application on a single Cheroot server.

.. _Cheroot: https://github.com/cherrypy/cheroot
