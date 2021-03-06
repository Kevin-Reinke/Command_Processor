# Command Processor - SA-MP

This include forwards in-game command input to hard-coded functions, at a fast speed.

Benchmarks with I-ZCMD (5 consecutive tests):

	This Include: 513 | 10/11/2016
	This Include: 522 | 10/11/2016
	This Include: 512 | 10/11/2016
	This Include: 508 | 10/11/2016
	This Include: 513 | 10/11/2016

	I-ZCMD: 1,111 | 10/08/2016
	I-ZCMD: 1,106 | 10/08/2016
	I-ZCMD: 1,105 | 10/08/2016
	I-ZCMD: 1,118 | 10/08/2016
	I-ZCMD: 1,115 | 10/08/2016

Exclusive benchmarks with Pawn.CMD, as Pawn.CMD's commands are truncated to 24 characters and this include's commands are truncated to 27 characters (5 consecutive tests):

	With Callbacks:

	This Include: 568 | 10/11/2016
	This Include: 571 | 10/11/2016
	This Include: 571 | 10/11/2016
	This Include: 570 | 10/11/2016
	This Include: 569 | 10/11/2016

	Pawn.CMD: 1,434 | 10/10/2016
	Pawn.CMD: 1,443 | 10/10/2016
	Pawn.CMD: 1,432 | 10/10/2016
	Pawn.CMD: 1,450 | 10/10/2016
	Pawn.CMD: 1,449 | 10/10/2016

	Without Callbacks:

	This Include: 533 | 10/11/2016
	This Include: 531 | 10/11/2016
	This Include: 528 | 10/11/2016
	This Include: 533 | 10/11/2016
	This Include: 532 | 10/11/2016

	Pawn.CMD: 484 | 10/10/2016
	Pawn.CMD: 497 | 10/10/2016
	Pawn.CMD: 484 | 10/10/2016
	Pawn.CMD: 487 | 10/10/2016
	Pawn.CMD: 491 | 10/10/2016

Old benchmarks of this include in comparison with Pawn.CMD as it updates:

	With Callbacks:

	This Include: 659 | 10/10/2016
	This Include: 658 | 10/10/2016
	This Include: 663 | 10/10/2016
	This Include: 661 | 10/10/2016
	This Include: 664 | 10/10/2016

	Without Callbacks:

	This Include: 624 | 10/10/2016
	This Include: 626 | 10/10/2016
	This Include: 623 | 10/10/2016
	This Include: 623 | 10/10/2016
	This Include: 624 | 10/10/2016

Old benchmarks of this include as it updates (Code Used: This Include | I-ZCMD):

	This Include: 613 | 10/10/2016
	This Include: 610 | 10/10/2016
	This Include: 613 | 10/10/2016
	This Include: 617 | 10/10/2016
	This Include: 612 | 10/10/2016

	This Include: 841 | 10/10/2016
	This Include: 848 | 10/10/2016
	This Include: 843 | 10/10/2016
	This Include: 846 | 10/10/2016
	This Include: 850 | 10/10/2016

	This Include: 866 | 10/09/2016
	This Include: 861 | 10/09/2016
	This Include: 864 | 10/09/2016
	This Include: 865 | 10/09/2016
	This Include: 863 | 10/09/2016

	This Include: 882 | 10/08/2016
	This Include: 893 | 10/08/2016
	This Include: 878 | 10/08/2016
	This Include: 883 | 10/08/2016
	This Include: 892 | 10/08/2016

Code used to benchmark this include and I-ZCMD (just note the changes in the arguments of OnPlayerCommandPerformed):

	// ** MAIN

	main()
	{
		new tick = GetTickCount();
		for(new i = 0; i < 50000; i ++)
		{
			OnPlayerCommandText(0, "/AaaaaaaaaaaaaaaAaaaaaaaaAa6 test");
			OnPlayerCommandText(0, "/AaaaaaaaaaaaaaaAaaaaaaaaAa6 test");
			OnPlayerCommandText(0, "/AaaaaaaaaaaaaaaAaaaaaaaaAa6 test");
		}

		printf("%d", GetTickCount() - tick);
	}

	// ** CALLBACKS

	public OnGameModeInit()
	{
		return 1;
	}

	public OnGameModeExit()
	{
		return 1;
	}

	public OnPlayerCommandPerformed(playerid, cmd[], params[], success)
	{
		/*if(success)
		{
			print("success true");
		}
		else
		{
			print("success false");
		}*/
		return 1;
	}

	// ** COMMANDS

	CMD:aaaaaaaaaaaaaaaaaaaaaaaaaa6(playerid, params[])
	{
		//printf("%d %s", playerid, params);
		return 1;
	}

Code used to benchmark this include with Pawn.CMD:

	// ** INCLUDES

	#include <a_samp>
	#include <sscanf2>
	#include <command_processor>

	// ** MAIN

	main()
	{
		print("Loaded \"command_processor_benchmark.amx\".");
	}

	// ** CALLBACKS

	public OnGameModeInit()
	{
		return 1;
	}

	public OnGameModeExit()
	{
		return 1;
	}

	public OnPlayerConnect(playerid)
	{
		new tick = GetTickCount();
		for(new i = 0; i < 50000; i ++)
		{
			OnPlayerCommandText(playerid, "/AaaaaaaaaaaaAaaaaaaaaAa6 test");
			OnPlayerCommandText(playerid, "/AaaaaaaaaaaaAaaaaaaaaAa6 test");
			OnPlayerCommandText(playerid, "/AaaaaaaaaaaaAaaaaaaaaAa6 test");
		}

		printf("%d", GetTickCount() - tick);
		return 1;
	}

	public OnPlayerCommandReceived(playerid, cmd[], params[])
	{
		return 1;
	}

	public OnPlayerCommandPerformed(playerid, cmd[], params[], success)
	{
		/*if(success)
		{
			print("success true");
		}
		else
		{
			print("success false");
		}*/
		return 1;
	}

	// ** COMMANDS

	command(aaaaaaaaaaaaaaaaaaaaaaa6, playerid, params[])
	{
		//printf("%d %s", playerid, params);
		return 1;
	}
	
Code used to benchmark Pawn.CMD with this include (please note that PC_EmulateCommand does not fully execute if the player isn't connected):

	// ** INCLUDES

	#include <a_samp>
	#include <Pawn.CMD>

	// ** MAIN

	main()
	{
		print("Loaded \"command_processor_pawncmd_benchmark.amx\".");
	}

	// ** CALLBACKS

	public OnGameModeInit()
	{
		return 1;
	}

	public OnGameModeExit()
	{
		return 1;
	}

	public OnPlayerConnect(playerid)
	{
		new tick = GetTickCount();
		for(new i = 0; i < 50000; i ++)
		{
			PC_EmulateCommand(playerid, "/AaaaaaaaaaaaAaaaaaaaaAa6 test");
			PC_EmulateCommand(playerid, "/AaaaaaaaaaaaAaaaaaaaaAa6 test");
			PC_EmulateCommand(playerid, "/AaaaaaaaaaaaAaaaaaaaaAa6 test");
		}

		printf("%d", GetTickCount() - tick);
		return 1;
	}

	public OnPlayerCommandReceived(playerid, cmd[], params[], flags) 
	{
		return 1;
	}

	public OnPlayerCommandPerformed(playerid, cmd[], params[], result, flags)
	{
		/*if(result)
		{
			print("result true");
		}
		else
		{
			print("result false");
		}*/
		return 1;
	}

	// ** COMMANDS

	CMD:aaaaaaaaaaaaaaaaaaaaaaa6(playerid, params[])
	{
		//printf("%d %s", playerid, params);
		return 1;
	}

Callbacks:

	forward OnPlayerCommandReceived(playerid, cmd[], params[]);
	forward OnPlayerCommandPerformed(playerid, cmd[], params[], success);

Other:

	- isnull macro will be defined by the include

The example script (command_processor_example.pwn) and the include (command_processor.inc) requires:
* SSCANF: http://forum.sa-mp.com/showthread.php?t=602923
