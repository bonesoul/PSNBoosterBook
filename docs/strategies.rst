.. _strategies:

===============
Strategies
===============

``Strategies`` are basically executed by ``The Brain Engine`` to calculated expected outcome & profit for a given set of actions. ``The Brain Engine`` can run 3 kind of strategies;

- Configuration based strategies
- Programmatic strategies
- On-the-fly strategies.

Configured
==========

``Configuration based strategies`` are simple ``YAML markup`` based strategies. Although it has a pretty basic syntax, it can
easly handle the most basic tasks for creating simple strategies.

.. code-block:: yaml

	name: TR markets - BTC
	description: BTC arbitrage strategy for Turkish markets
	region: TR
	author: Bonesoul
	version: 1.0
	enabled: true

	defaults:
	  input: &input 1000 TRY # default input amount

	variants:

		- name: Koinim > Paribu
		  description: Buy BTC [Koinim] > Sell BTC [Paribu]
		  exec:	
			- cashin: # Cash in TRY to Koinim.
			  - market: Koinim 
			  - amount: *input
			- buy: BTC # Buy BTC using TRY.
			- transfer: Paribu # Transfer Btc to Paribu.
			- sell: TRY # Sell BTC for TRY.
			- cashout: # Cash out TRY.	

The above strategy will basically execute these actions;

- The above strategy will basically cash in ``1000 USD`` to Bitcoin exchange called ``Koinim``.
- Will buy ``Bitcoin`` with all it's fiat.
- Transfer ``Bitcoins`` to another exchange called ``Paribu``.
- Will sell all the ``Bitcoins``.
- Will cash out from the market.

Programmatic
============

``Programmatic strategies`` are custom developed strategies by the user with ``C#`` or any other ``.Net based`` language. They are far more
powered compared to ``Configuration based strategies`` as the user will be able to develop virtually every possible trading & arbitrage strategy.

As an example, the code below executes the same above strategy;

.. code-block:: c#

	using System;
	using System.Collections.Generic;
	using System.Collections.ObjectModel;
	using Brain.Framework.Finances.Currencies;
	using Brain.Framework.Finances.Generic;
	using Brain.Framework.Strategies.Programmatic;
	using Brain.Framework.Strategies.Variants;
	using Brain.Framework.Strategies.Variants.Programmatic;

	namespace Brain.Strategies
	{
		[Serializable]
		public class Strategy : IProgrammaticStrategy
		{
			public string Name { get; }

			public string Author { get; }

			public IReadOnlyCollection<IVariant> Variants { get; }

			[NonSerialized]
			private readonly IList<IVariant> _variants;

			private readonly FiatCurrency _try = new FiatCurrency("TRY");

			private readonly CoinCurrency _btc = new CoinCurrency("BTC");

			public Strategy2()
			{
				Name = "TR markets - BTC";
				Author = "Bonesoul";

				_variants = new List<IVariant>();
				Variants = new ReadOnlyCollection<IVariant>(_variants);
			}

			public void Load()
			{
				var variant = new ProgrammaticVariant(this, "Koinim -> Paribu");
				variant1.Execute = new Action(() =>
				{
					variant.CashIn("Koinim", new Amount(1000, _try));
					variant.Buy(_btc);
					variant.Transfer("Paribu");
					variant.Sell(_try);
					variant.CashOut();
				});

				_variants.Add(variant);
			}
		}
	}
	
Yet again it will execute the same actions;

- The above strategy will basically cash in ``1000 USD`` to Bitcoin exchange called ``Koinim``.
- Will buy ``Bitcoin`` with all it's fiat.
- Transfer ``Bitcoins`` to another exchange called ``Paribu``.
- Will sell all the ``Bitcoins``.
- Will cash out from the market.

On-the-fly
==========

``On-the-fly strategies`` are strategies that are not created by the user but ``The Brain Engine`` itself based on current market oppurtunities.
Once the market data is read and processed by ``data analyzers``, a ``oppurtunity analyzer`` that can evaluate possible profit opportunities within the markets and if so, it can dynamically create a suitable ``on-the-fly strategy`` for it.

The basic workflow for ``On-the-fly strategies`` is;

* ``The Brain Engine`` will read data from markets.
* Available ``data analyzers`` will be executed where they will process the market data.
* ``Opportunity analyzers`` will be evaluating the outcome from ``data analyzers``. Once an opportunity is found, it'll present the user percentage profit and create a ``on-the-fly strategy`` utilizing it to exactly calculate the expected outcome.