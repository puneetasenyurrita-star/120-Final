# 120-Final
This final project will show my audience of peers and instructor that I incorporate math programming and software to gain insight in analyzing music data through different comparisons.


import os
import sys

# Check if running in Google Colab
try:
    import google.colab
    IN_COLAB = True
    print("Running in Google Colab")
    
    # Clone repository if in Colab
    if not os.path.exists('/content/math120_final_project/'):
        !git clone https://github.com/MBanuelos/math120_final_project.git
    
    # Change to project directory
    os.chdir('/content/math120_final_project')
    
except ImportError:
    IN_COLAB = False
    print("Running locally")

# Add src directory to Python path
if 'src' not in sys.path:
    sys.path.append('src')

print(f"Current working directory: {os.getcwd()}")
