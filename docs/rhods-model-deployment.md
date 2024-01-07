# Configure Model in Jupyter notebook 



**1. Accessing the Jupyter Notebook Environment**

   - **Locate the RHODS URL:**
      - Navigate to the `redhat-ods-applications` project.
      - Find the RHODS URL under the "Routes" section.: 
      - ![20240106140411](https://i.imgur.com/8gCwsbd.png)
   - **Log in and launch Jupyter:**
      - Log in to the RHODS environment using the provided URL.
      - Click on the "Launch application" button under the "Jupyter" tile.:
      - ![20240106141839](https://i.imgur.com/haOGoij.png)
      - Select "PyTorch 2023.2" and "Container Size Small" from the options.
      - Click on "Launch" and wait for the Jupyter notebook environment to load. ![20240106142158](https://i.imgur.com/1b2xsGJ.png)
   - **Open a new tab:**
      - Once the environment is ready, click on "Open in new Tab" to start working. Image of opening Jupyter notebook in a new tab:   
![20240106195324](https://i.imgur.com/LLR8YFO.png)

**2. Cloning the Repository**

   - **Copy and paste the repository link:** In the notebook environment, enter the following command in a code cell:
     ```bash
     git clone https://github.com/bdherouville/redhat-edge-ai-industrial-demo.git
     ```
      - Execute the cell to clone the repository.:
      - ![20240106195630](https://i.imgur.com/FFk4oSk.png)

**3. Opening the Notebook**

   - **Navigate to the notebook:**
      - Within the cloned repository, locate and open the notebook named `Train_Nut_OpenShift.ipynb`. ![20240106195708](https://i.imgur.com/8UNTyVl.png)
      - You're now ready to work with the notebook!  
    ![20240106195746](https://i.imgur.com/WkWAnSz.png)
