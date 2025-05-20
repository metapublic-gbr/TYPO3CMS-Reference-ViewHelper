:navigation-title: translate

..  include:: /Includes.rst.txt
..  _typo3-fluid-translate:

====================================
Translate ViewHelper `<f:translate>`
====================================

..  typo3:viewhelper:: translate
    :source: /Global.json
    :display: tags,description,gitHubLink
    :noindex:

..  seealso::
    *   `Working with XLIFF files <https://docs.typo3.org/permalink/t3coreapi:xliff-api>`_
    *   `Multi-language Fluid templates <https://docs.typo3.org/permalink/t3coreapi:extension-localization-fluid>`_

..  contents:: Table of contents

..  _typo3-fluid-translate-example:

Examples
========

..  _typo3-fluid-translate-example-key-only:

Short key - Usage in Extbase
----------------------------

The :ref:`key <t3viewhelper:viewhelper-argument-typo3-cms-fluid-viewhelpers-translateviewhelper-key>`
argument alone works only within the Extbase context.

..  tabs::

    ..  group-tab:: Fluid

        ..  code-block:: html

            <f:translate key="domain_model.title" />

    ..  group-tab:: Language file example

        ..  literalinclude:: _Translate/_locallang.xlf
            :language: xml
            :caption: packages/my_extension/Resources/Private/Language/locallang.xlf

Alternatively, you can use `id` instead of `key`, producing the same output.
If both `id` and `key` are set, `id` takes precedence.

If the translate ViewHelper is used with only the language key, the template
**must** be rendered within an Extbase request, usually from an Extbase
controller action. The default language file must be located in the same
extension as the Extbase controller and must be saved in the file
:file:`Resources/Private/Language/locallang.xlf`.

..  _typo3-fluid-translate-example-key-extension:

Short key and extension name
----------------------------

When the ViewHelper is called from outside Extbase, it is mandatory to either
pass the extension name or use the full `LLL:` reference for the key.

..  code-block:: html

    <f:translate key="domain_model.title" extensionName="my_extension" />

The default language file must be saved in the extension specified in the
argument, in the file :file:`Resources/Private/Language/locallang.xlf`.

..  _typo3-fluid-translate-example-key-full:

Full `LLL:` reference key
-------------------------

Using the `LLL:` language reference syntax, a language key from any `.xlf`
file can be used.

..  code-block:: html

    <f:translate
        key="LLL:EXT:my_extension/Resources/Private/Language/my_locallang.xlf:domain_model.title"
    />

..  _typo3-fluid-translate-example-html:

Keeping HTML tags within translations
-------------------------------------

HTML in language labels can be used when you surround the translate ViewHelper
with a ViewHelper that allows HTML output, such as the
`Sanitize.html ViewHelper <f:sanitize.html> <https://docs.typo3.org/permalink/t3viewhelper:typo3-fluid-sanitize-html>`_
in the backend or the `Format.html ViewHelper <f:format.html> <https://docs.typo3.org/permalink/t3viewhelper:typo3-fluid-format-html>`_
in the frontend.

..  tabs::

    ..  group-tab:: Fluid

        ..  code-block:: html

            <f:format.html>
                <f:translate
                    key="LLL:EXT:my_extension/Resources/Private/Language/my_locallang.xlf:myKey"
                />
            </f:format.html>

    ..  group-tab:: Language file example

        ..  literalinclude:: _Translate/_locallang_html.xlf
            :language: xml
            :caption: packages/my_extension/Resources/Private/Language/locallang.xlf

The entry in the `.xlf` language file must be surrounded by `<![CDATA[...]]>`.

..  warning::
    When allowing HTML tags in translations, ensure all translators are
    educated about `Cross-site scripting (XSS) <https://docs.typo3.org/permalink/t3coreapi:security-xss>`_.

..  _typo3-fluid-translate-example-placeholder:

Displaying translated labels with placeholders
----------------------------------------------

..  tabs::

    ..  group-tab:: Fluid

        ..  code-block:: html

            <f:translate
                key="LLL:EXT:my_extension/Resources/Private/Language/locallang.xlf:domain_model.title"
                arguments="{0: '{myVariable}'}"
            />

            Or with several numbered placeholders:

            <f:translate
                key="LLL:EXT:my_extension/Resources/Private/Language/locallang.xlf:domain_model.title"
                arguments="{1: '{myVariable}', 2: 'Some String'}"
            />

    ..  group-tab:: Language file example

        ..  literalinclude:: _Translate/_locallang_arguments.xlf
            :language: xml

The placeholder `%s` is used to insert dynamic values, passed as the parameter
:ref:`arguments <t3viewhelper:viewhelper-argument-typo3-cms-fluid-viewhelpers-translateviewhelper-arguments>`
to the translate ViewHelper. If `%s` appears multiple times, it references
subsequent array entries in order.

To avoid dependency on order, use indexed placeholders like `%1$s`. This
explicitly refers to the first value and ensures that it is treated as a string
(`$s`). This method is based on PHP’s
`vsprintf function <https://www.php.net/manual/en/function.vsprintf.php>`,
which allows for more flexible and readable formatting.

..  _typo3-fluid-translate-example-inline:

Inline notation with arguments and default value
------------------------------------------------

Like all other ViewHelpers, the translate ViewHelper can be used with the
`Inline syntax <https://docs.typo3.org/permalink/fluid:viewhelper-inline-notation>`_.

..  code-block:: html

    {f:translate(key: 'someKey', arguments: {0: 'dog', 1: 'fox'}, default: 'default value')}

This is especially useful if you want to pass the translated label to another
ViewHelper, for example, as the `alt` parameter of an image:

..  code-block:: html

    <f:image src="EXT:my_extension/Resources/Public/icons/Edit.svg"
        alt="{f:translate(key: 'icons.edit', default: 'Edit')}"
    />

..  _typo3-fluid-translate-example-console:

The translation ViewHelper in console/CLI context
-------------------------------------------------

The translation ViewHelper determines the target language based on the current
frontend or backend request.

If you are rendering your template from a console/CLI context, for example,
when you `Send emails with FluidEmail <https://docs.typo3.org/permalink/t3coreapi:mail-fluid-email>`_,
use the argument :ref:`languageKey <t3viewhelper:viewhelper-argument-typo3-cms-fluid-viewhelpers-translateviewhelper-languagekey>`
to specify the desired language for the ViewHelper:

..  code-block:: html

    Salutation in Danish:
    <f:translate
        key="LLL:EXT:my_extension/Resources/Private/Language/my_locallang.xlf:salutation"
        languageKey="da"
    />

    Salutation in English:
    <f:translate
        key="LLL:EXT:my_extension/Resources/Private/Language/my_locallang.xlf:salutation"
        languageKey="default"
    />

..  _typo3-fluid-translate-arguments:

Arguments of the f:translate ViewHelper
=======================================

..  typo3:viewhelper:: translate
    :source: /Global.json
    :display: arguments-only
