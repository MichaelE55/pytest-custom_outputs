=====================
pytest-custom_outputs
=====================

.. image:: https://img.shields.io/pypi/v/pytest-custom_outputs.svg
    :target: https://pypi.org/project/pytest-custom_outputs
    :alt: PyPI version

.. image:: https://img.shields.io/pypi/pyversions/pytest-custom_outputs.svg
    :target: https://pypi.org/project/pytest-custom_outputs
    :alt: Python versions

.. image:: https://github.com/MichaelE55/pytest-custom_outputs/actions/workflows/main.yml/badge.svg
    :target: https://github.com/MichaelE55/pytest-custom_outputs/actions/workflows/main.yml
    :alt: See Build Status on GitHub Actions

A plugin that allows users to create and use custom outputs instead of the standard Pass and Fail

----

This `pytest`_ plugin was generated with `Cookiecutter`_ along with `@hackebrot`_'s `cookiecutter-pytest-plugin`_ template.


Features
--------

- Useful for if you want more than just the default Pass, Fail, and Skip outcomes.
- Allows for the creation and usage of custom outputs.
- Custom outputs are filled out in a file called "pytest_custom_outputs.json".
- Custom outputs are customizable in their name, description, result code, tag, and color.
- Supports the creation of an unknown output. If no result matches a default or custom output, then it will be categorized as an unknown output.
- Unknown outputs are also fully customizable.


Requirements
------------

None


Installation
------------

You can install "pytest-custom_outputs" via `pip`_ from `PyPI`_::

    $ pip install pytest-custom_outputs


Usage
-----

In the directory where you will be running your pytest, create a file called "pytest_custom_outputs.json" or bring one over from the example folder.
You will use this file to create your own custom outputs.

Please refer to the file in the example folder on how to maintain the proper structure

use_unknown_if_no_match -> If True, use the unknown output below if there is no match. Otherwise, use standard skip
unknown -> The output to use if a test's result is not in default or custom outputs 
custom_outputs -> A dictionary with all the custom outputs you write inside of it. You can edit, delete, and add new outputs here.

Each custom output is denoted by a name. The name is also the key for that output
For example, in the above example file, "Pass_with_exception" and "Fatal_failed" are the names for their respective output.
Names are also how we determine the result of a test case. 
This is done by using skip followed by the name in the parameter.

For example:
        import pytest
        from pytest import skip

        def test_1():
                skip("Pass_with_exception")


In the example above, test_1 will result in "passed_with_exception".
Because the name overrides the outcome, it will not result in a skip.
We use the keyword skip as a means to obtaining out desired outcome.

If we put a name that is not in our custom output in the skip parameter,
then
        if we set unknown to True in the json, we will use the unknown outcome
        else we will use the default skip and pass the name as a message (Standard skip behavior)


The rest of the information in the json file can be edited and customized to your liking. 


Contributing
------------
Contributions are very welcome. Tests can be run with `tox`_, please ensure
the coverage at least stays the same before you submit a pull request.

License
-------

Distributed under the terms of the `BSD-3`_ license, "pytest-custom_outputs" is free and open source software


Issues
------

If you encounter any problems, please `file an issue`_ along with a detailed description.

.. _`Cookiecutter`: https://github.com/audreyr/cookiecutter
.. _`@hackebrot`: https://github.com/hackebrot
.. _`MIT`: https://opensource.org/licenses/MIT
.. _`BSD-3`: https://opensource.org/licenses/BSD-3-Clause
.. _`GNU GPL v3.0`: https://www.gnu.org/licenses/gpl-3.0.txt
.. _`Apache Software License 2.0`: https://www.apache.org/licenses/LICENSE-2.0
.. _`cookiecutter-pytest-plugin`: https://github.com/pytest-dev/cookiecutter-pytest-plugin
.. _`file an issue`: https://github.com/MichaelE55/pytest-custom_outputs/issues
.. _`pytest`: https://github.com/pytest-dev/pytest
.. _`tox`: https://tox.readthedocs.io/en/latest/
.. _`pip`: https://pypi.org/project/pip/
.. _`PyPI`: https://pypi.org/project
