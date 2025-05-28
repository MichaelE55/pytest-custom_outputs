=====================
Pytest Custom Outputs
=====================

.. image:: https://img.shields.io/pypi/v/pytest-custom_outputs.svg
    :target: https://pypi.org/project/pytest-custom_outputs
    :alt: PyPI version

.. image:: https://img.shields.io/pypi/pyversions/pytest-custom_outputs.svg
    :target: https://pypi.org/project/pytest-custom_outputs
    :alt: Python versions

With pytest-custom_outputs, you can create expressive and informative custom test results that go beyond the standard pass/fail binary. You can adapt outcomes and messages to your project's specific requirements and get more informative insights from your test runs. You can send messages alongside your outputs to better share your thoughts on each test case.

pytest-custom_outputs also provides an interface that allows you to access your test results from within your fixtures. Even if you don't want to make custom outputs, this plugin will still be useful if you want to collect your test information after each function. For example:

- You can send an API call with the result status and message right after your test function.
- You can attach the result status and message to any logs.
- You can pass the result status and message as an argument to any function.

(When functions fail, the assert message / reason for failure will automatically attach itself as the message.)


Features
--------

- **Flexible Output Types:** Define new outcome types like "unimplemented", "soft_fail"," "inconclusive," or any custom label that suits your testing needs.
- **Fully Customizeable:** Custom outputs are customizable in their name, description, result code, tag, and color.
- **Seamless Integration:** Easily incorporate custom outputs into your existing pytest test suites.
- **Terminal and File Reporting:** View your custom outputs in both your terminal output and pytest file reports.
- **Improved Communication:** Attach messages alongside your outputs to further share details on your test results.
- **Enhanced Error-Catching:** Failed test cases now automatically attach the associated error as a message to the result.
- **Retrieve Detailed Results:** Access comprehensive information about each test, including the status (passed, failed, skipped, or any of your custom outputs) and the attached message from within your fixtures.


Installation
------------
.. code-block:: bash

    pip install pytest-custom_outputs

Setup Custom Outputs
--------------------
This guide will help you on how to create and declare your own outputs.

First, in the directory where you will be running your pytest, create a file called pytest_custom_outputs.json. You will use this file to create your own custom outputs. The plugin makes a check for this filename, so it has to be named exactly as above. Feel free to copy and paste the below json file into yours and edit from there.

pytest_custom_outputs.json

.. code-block:: python

    {
        "custom_outputs": {
                "Pass_with_exception": {
                        "desc":"passed_with_exception",
                        "code":"P",
                        "tag":"XPASSED",
                        "color":"green"
                },
                "Fatal_failed": {
                        "desc":"fatal_failed",
                        "code":"!",
                        "tag":"FAILED",
                        "color":"red"
                },
                "Not_available": {
                        "desc":"not_available",
                        "code":"N",
                        "tag":"NOT_AVAILABLE",
                        "color":"blue"
                },
                "Failed_but_proceed": {
                        "desc":"failed_but_proceed",
                        "code":"X",
                        "tag":"FAILED_BUT_PROCEED",
                        "color":"red"
                },
                "Unimplemented": {
                        "desc":"unimplemented",
                        "code":"U",
                        "tag":"UNIMPLEMENTED",
                        "color":"yellow"
                },
                "Skipped": {
                        "desc":"skipped",
                        "code":"S",
                        "tag":"SKIPPED",
                        "color":"yellow"
                }
          }
    }

**custom_outputs** - The dictionary with all the custom outputs inside of it. You can edit, delete, and add new outputs here.

**"Pass_with_exception"** - An example custom output. When you want to assert this outcome in your test, you call it by this name.

**desc** - Description of the custom output. This is what gets outputted when pytest ends. Disclaimer: The description names "passed" and "failed" are both taken by standard pytest. So it is recommended not to use these for your custom outputs

**code** - The custom output's code. This is what gets outputted when a specific test ends.

**tag** - The tag associated with the custom output.

**color** - The color of the custom output.

You can add to or edit this file as much as you want to have it conform to your testing needs.

Use Custom Outputs
------------------
Once you've finished setting up your custom outputs in the previous section, you can start using them in your tests.

Use the provided c_assert function to return your output. c_assert takes one required parameter (status) and one optional parameter (message). In the status field, you write down the name of the output you wish to return. In the optional message field, you can add a message to accompany your status.

example_1.py

.. code-block:: python

    import pytest
    from pytest_custom_outputs import c_assert

    def test_1():
        c_assert("Pass_with_exception")

In the example above, test_1 will result in "passed_with_exception".

example_2.py

.. code-block:: python

    import pytest
    from pytest_custom_outputs import c_assert

    def test_2():
        c_assert("Failed_but_proceed", "The test failed section X but continue anyways")

In the example above, test_2 will result in "failed but proceed" and also accompany the message alongside it.

If we put a name that is not in our custom output in the c_assert parameter, then it will assert the unknown outcome Because of this, it is recommended to not make a custom output with the name unknown

The rest of the information in the json file can be edited and customized to your liking.

Attach Messages
---------------
If you use c_assert, you can send the message through the second argument as seen in the previous example.

But if you wish to attach messages to the normal assert function, then you must use the attach function. Keep in mind that this is a setter function and using it multiple times will result in only saving the latest message.

Adding messages to your asserts will automatically be attached if the assertion fails. So the attach function is only helpful when you wish to add information alongside a passing test function.

example_1.py

.. code-block:: python

    import pytest
    from pytest_custom_outputs import attach
    
    def test_1():
        attach("This test ran well, X received 2/3 responses")
        assert True

example_2.py

.. code-block:: python

    import pytest
    from pytest_custom_outputs import attach
    
    def test_1():
        attach("Test failed because servers did not receive any responses")
        assert False

Example 2 could also be rewritten as:

example_2_rewritten.py

.. code-block:: python

    import pytest
    from pytest_custom_outputs import attach

    def test_1():
        assert False, "Test failed because servers did not receive any responses"

Access Results
--------------
This feature works regardless of whether or not you use custom_outputs.

Within your fixture, use the provided get_results function to get the results from the current test. get_results takes one required parameter and that is request. get_results only works as intended in function-scoped fixtures and AFTER the yield function is called

conftest.py

.. code-block:: python

    import pytest
    from pytest_custom_outputs import get_results

    @pytest.fixture(scope='function', autouse=True)
    def my_fixture(request):
        yield
        print(get_results(request))

In the example above, after each test is done, their result status gets printed, and if there is an attached message, that gets printed too. This works with all methods of returning results in pytest. If the test fails for any reason, the error will automatically attach itself to the message too. This can help with differentiating the Fail results based on the error which caused it.


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
