# Compute-Canada-Setup-Guide
# Compute Canada With Jupyter Lab

1. Download MobaXtrem at first.

2. After installing it, click on the Start the local terminal
   
3. Now in the terminal, login to the Compute Canada following the below steps:
    a. Login: ssh -Y <your-username>@graham.computecanada.ca
       Example: ssh -Y rumman@graham.computecanada.ca
    b. Enter your password at the prompt.
    c. After entering the correct credentials you will be able to see the following page.
    d. If “ls” is a common being type then it will show all the folders in that directory.
4. Now write the below commands one by one which will lead you to your folder. As my name is rumman that is why I went inside to that folder
5. Now here, git clone https://github.com/tvhahn/compute-canada-hpc.git write this command which will help cloning the repository whatever you want.
6. Now for creating the virtual environment you need to follow the below steps:
   a. Go to your home directory. The cd command takes you to your home directory.
   b. Load Python/3.10.13 .: module load python/3.10.13 ( whichever is latest for you).
   c. Create the virtual environment in your home directory: virtualenv ~/jupyter1
   d. Activate the virtual environment you just created: source ~/jupyter1/bin/activate
7. One can install python packages here following the below steps:
   a. Install the packages you need to open up a Jupyter notebook and do data analysis.
   b. While the jupyter1 environment is active, upgrade the package manager, pip: pip install --no-index --upgrade pip You     
   c. should always do this when setting up a new environment.
   d. Install basic data-science packages, scikit-learn, Pandas, Matplotlib: pip install --no-index pandas scikit_learn matplotlib seaborn
8. To launch jupyter lab you need to follow the below steps:
   a. Use nano (text editor in linux) to create a bash script that we'll call upon to open up a Jupyter Lab session.
   b. Create a script in your virtual environment (make sure jupyter1 is active), in the bin folder: nano $VIRTUAL_ENV/bin/notebook.sh
   c. This opens up the nano text editor, so that we can create the bash script
   d. Copy the following code in that file

      #!/bin/bash
      unset XDG_RUNTIME_DIR
      jupyter-lab --ip $(hostname -f) --no-browser
   
   e. Press ctrl-O to save, and after that press enter and ctrl-X to exit.
   f. Back in your home directory, change the user privileges of the notebook.sh that you just created (we'll allow the user, u, to execute, x, the file). This is needed so that we can run the script in the bin folder: chmod u+x $VIRTUAL_ENV/bin/notebook.sh
9. To run jupyter lab create an allocation by following the below steps:
   a. While in your virtual environment, run the following:
    salloc --time=1:0:0 --ntasks=1 --cpus-per-task=4 --mem-per-cpu=2048M --account=<your-account> srun $VIRTUAL_ENV/bin/notebook.sh
   b. Rename account name with your Compute Canada account name
   c. In the above command, 1 hour allocated for 1 task, using 4 cpus and 2048 MB of RAM/CPU. Allocated on the
      Warning: Try not to allocate more than you need so that the resources can be efficiently used between users.
   d. After running that command you will be able to see the following thing
10. Now open another new terminal side by side
11. In the new terminal, ssh into the graham server. Type something like this, based on what is shown the other terminal you have open showing the notebook access token: ssh -L 8888:[gra800.graham.sharcnet:8888(change this with new value)] rumman@graham.computecanada.ca
12. Replace the red one with the value marked as yellow which you got from the previous terminal
13. Now if you see the following picture in your new terminal, then your task is almost done:
14. Then on local browser, copy the link to Jupyter lab with the access token, like: http://localhost:8888/?token=<token>
15. Now, Replace the token value with previous terminal’s token value which has been marked yellow in the below picture:
16. Now you will be able to have the access to your server
17. Now you will have the access to your server
18. Now for the scheduling jobs follow the following steps:
  a. Now create a file (ex: random_search_cpu.sh)
  b. Inside this file add the following bash script

      #!/bin/bash
      #SBATCH --account=<your-account>
      #SBATCH --cpus-per-task=4   # number of cores
      #SBATCH --mem=4G            # memory for the entire job across all cores (4GB)
      #SBATCH --time=0-00:10      # time (DD-HH:MM)
      #SBATCH --output=%N-%j.out  # %N for node name, %j for jobID
      #SBATCH --mail-type=ALL               # Type of email notification- BEGIN,END,F$
      #SBATCH --mail-user=<your-email>  # Email to which notifications will be $
      
      
      module load python/3.10.13
      source ~/jupyter1/bin/activate
  
      python train_model_tcn.py

   c. The email id is provided so that when this will start and finish it will be sent to the provided mail id.
   d. Account name will be replaced by your own account name, time can be updated with requirement and email id.
   e. For submitting the bash job do the following command: 
      sbatch random_search_cpu.sh
   f. To see the progress of the bash job type the following command: 
squeue -u rumman

Referenced Video: https://www.youtube.com/watch?v=K8wuaIKW6aU



# Compute Canada With Visual Studio

1. Download Visual Studio Code Community edition
2. Now go to visual studio code. Need to add the following extensions:
    a. Click on this extension tab
    b. Now search remote and download the following extensions
3. Next step is to generate a public key and add that key in Compute Canada as well.
    a. Enter the following command which will result in creating the public key. At the end, add your own email address
ssh-keygen -t rsa -b 4096 -C ali5i@uwindsor.ca
    b. It will ask for the location where you want to save the key pair. If you press enter then C:\Users\Rumman Ali/.ssh/id_rsa this will be its default location.
    c. And be careful about the passphrase, if you are providing one then do remember it.
    d. With the following command, you can see your public key in the command line or you open it from the folder as well.
Command is: cat ~/.ssh/id_rsa.pub
    e. Log into Compute Canada. After logging in go to the Manage SSH Keys
    f. Paste the public key in the SSH Key section. After pasting the public key in that box. Now, click on the button Add Key which will result in adding the key.
4. Now go to Visual Studio Code and open Command Palette from view or by using shower key Ctrl+Shift+P
5. Now after that go to Remote SSH: Open SSH Configuration file
6. Now just go with the default one:
7. Now in that file add the following things:
   Add the value with your own values. After that in the IdentifyFile just add the location of your public key in the quotation mark.
8. Now, again open Command Palette select the Remote-SSH: Connect to Host
9. Now, Select the Host you provided
10. Now it will redirect you to the new visual studio code. And you will have to provide your passphrase
11. Now click on the details. Otherwise it will keep on running.
12. Now provide the passcode for your two step verification
13. Now, after the successful connection, you will be able to see the following things
14. Now, if you open your new terminal in the VS code then you will be able to see that, you are now connected with your Compute Canada server
15. Now go to file and open folder
16. After that, it will ask you to redirect to the folder you want to work on.
17. And after selecting the folder, now you can write your code and run your file.

Referenced Video: https://www.youtube.com/watch?v=lKXMyln_5q4
   
