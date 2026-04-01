The Game
Practice your Game Hacking skills.

Task1: The Game
What is the Flag?
Answer: THM{I_CAN_READ_IT_ALL}

Approach:

Unzip the downloaded files and start the game Tetrix, then you will notice that you have to score 99999.. to get the Flag.
From here either you can open tha game in a GameEngine and edit that particulat function to get the Flag. The way i followed is,

strings "Tetrix.exe" | grep "THM{"

To get the flag.