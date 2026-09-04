===================
scrapy-agent-plugin
===================

An agent plugin that improves Scrapy support in coding agents:

* a ``scrapy`` skill with instructions for working on Scrapy projects;
* a `Scrapy MCP server`_ that allows agents to connect to running crawls and
  inspect them.

This repository contains plugins for the following standards, usable with the
same URL:

- `Claude Code <https://docs.claude.com/en/docs/claude-code/plugins>`__
- `Agent Plugins`_

Installation
============

Claude Code
-----------

Add this repository as a plugin marketplace and install the plugin from it::

    /plugin marketplace add https://github.com/scrapy/scrapy-agent-plugin
    /plugin install scrapy@scrapy-agent-plugin

Agent Plugins standard
----------------------

Point any client that supports the `Agent Plugins`_ standard at this repository.

Requirements
============

The skill
---------

The ``scrapy`` skill supports working with projects using any modern Scrapy
versions.

The MCP server
--------------

The MCP server requires uv_ and works with crawls running Scrapy 2.19.0 and
higher. See `its documentation <Scrapy MCP server>`_ for more details.

.. _uv: https://docs.astral.sh/uv/
.. _Scrapy MCP server: https://github.com/scrapy/scrapy-mcp-official
.. _Agent Plugins: https://agent-plugins.org/
