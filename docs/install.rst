.. _about:

===============
About
===============

Why?
====

As trading hundreds of cryptocoins in tens of markets could be really hard for users, **The Brain** aims to help with decision taking by presenting the user best possible summarized information.

How?
====

**The Brain** consists of multiple parts for use; The UI and Engine. ``The Brain Engine`` is responsible for running everything behind the curtain including reading & analyzing market data and processing it with opportunity analyzers & strategies.

Market Data
===========

**The Brain** can read market data from hundres of possible cryptocoin exchanges. Yet again users can also develop their own market data reader programmatically and **The Brain** will be then executing it.

Yet again, **The Brain** can read from fiat conversion API's available through the web to
execute cross-fiat conversions.

Data Analyzers
==============

Once the market data from all available sources are read, ``The Brain Engine`` will then
will start executing the available data-analyzer modules.

A ``data analyzer`` is a basic module that can read, process and finally manipulate the data from markets.

As an example ``MergedMarketsForPairs`` data analyzer merges all currency pairs within the market data and will then find each market with best ask & bid prices.

Opportunity Analyzers
=====================

Once data analyzers are all processed, ``The Brain Engine`` will move on executing ``opportunity analyzers``.

An ``opportunity analyzers`` is a basically another type of module that has access to results from ``data analyzers``.

They can basically take the results and check for any outstanding **opportunities** for the current market state.

As an example, a simple ``opportunity analyzer`` can read from the previous ``MergedMarketsForPairs`` data analyzer's results and
calculate if any outstanding opportunities stand for available currency pairs, in which our case buying from the lowest ask market and
selling it back in highest bid market would be one.

As well as presenting the user percentage profit, the ``opportunity analyzer`` can then create a ``on-the-fly strategy`` which will be then later executed and calculate the exact outcome.

Strategies
==========

``Strategies`` are basically executed by ``The Brain Engine`` to calculated expected outcome & profit for a given set of actions. ``The Brain Engine`` can run 3 kind of strategies;

- Configuration based strategies
- Programmatic strategies
- On-the-fly strategies.