# Integrating FEAGI with MuJoCo
Mentored by Mohammad Nadji and Kevin Araujo
by Apostoli Karpouzis, Christopher Trent-Davis, Joel Prebish, and Deekshita Behara

In our project, we aim to create a bridge of communication between **FEAGI** and **MuJoCo** to explore the possibilities of what these powerful tools are capable of together. To do so, it is important to understand what both FEAGI and MuJoCo are, and are capable of. 

**FEAGI**
---
Our sponsors at Neuraville have developed the **Framework for Evolutionary Artificial General Intelligence, or FEAGI**. FEAGI is an open-source artificial intelligence (AI) model developed to closely simulate a biological human brain. Humans don't think the way traditional AI is engineered, which is heavily backed by math and algorithms, but rather with a very complex neural structure unlike any other current AI. We have a vast network of billions of neurons, each forming many unique synaptic connections. They exhibit neuroplasticity, a key concept mimicked in FEAGI. Neuroplasticity allows neurons to constantly adapt to new behavior and learn more efficiently, breaking synaptic connections and strengthening other ones to develop more complex and mature skills. The static algorithms that remain unchanged in more traditional AI, is the core disadvantage it has over the human brain. FEAGI, however, is developed to closely replicate this activity of the biological brain for machine learning, allowing for neuroplasticity, multiple cortical areas, refractory periods, and more. This is what sets it apart as a technology, and allows for exciting exploration of its boundless capabilities.

![brain image](https://drive.google.com/uc?id=1OmON8JxVTclrQNCezvB2I-MEYwqxgG0e)

**MuJoCo**
---
**MuJoCo, representing Multi-Joint Dynamics with Contact,** is an advanced physics simulator, and another impressive open-source software. MuJoCo was a clear choice when looking to further research of FEAGI's capabilities. It simulates models with very high accuracy in real-time, providing data on a large number of joints, contact points with the surface, impact forces, and the dynamics between all these points. In our project, we focused on the humanoid model for simulation. This model represents all the joints in a human and how they interact.

![humanoid image](https://drive.google.com/uc?id=16NrbTr3qqJKdwPV5h5bw0f3UTK-PATPh)

We were tasked with creating a Python controller to interact with both the FEAGI SDK and MuJoCo SDK, and to begin exploration of how Neuraville's artificial brain could take control of the human model. The purpose of this is to eventually make the humanoid stand, walk, and even be aware of its surroundings to dodge obstacles. 


## Development

To develop the bridge between FEAGI and MuJoCo, we began by creating a Python controller that could interact seamlessly with both packages. This required understanding and leveraging the APIs provided by each tool. In order to do this, we created a `controller.py` file. This script interacts with FEAGI and MuJoCo through their respective APIs to establish a communication link. The controller runs locally and connects to FEAGI, which operates within a Docker environment. Once both are running, the script initiates the MuJoCo viewer. The controller sends data to FEAGI, which can be visualized using the Brain Visualizer. This allowed us to integrate the neuroplastic learning capabilities of FEAGI with the humanoid model's movement and sensory inputs. 

Next, we focused on building a functional pipeline that allowed MuJoCo's physics simulation to relay sensory data, such as joint angles and contact forces, back to FEAGI. FEAGI would process this data and send motor commands to MuJoCo, completing the control loop. To ensure smooth integration, we created a system to translate FEAGI's neural outputs into MuJoCo-compatible actuator inputs.

A major breakthrough was implementing a basic standing posture for the humanoid. This involved tuning both FEAGI's neural response and the physical parameters of the humanoid model, ensuring stability while mimicking natural human posture. We also programmed a feedback loop where FEAGI could learn and adapt its control strategy, leading to gradual improvements in maintaining balance and responding to simulated environmental changes.

Throughout development, we continually debugged and refined our approach, ensuring the FEAGI-MuJoCo integration worked efficiently and in real time. The culmination of this work was a humanoid capable of simple motor actions driven by an artificial brain, paving the way for more complex behaviors.

## User Guide

This section provides a step-by-step guide to using the software. 
Before starting, ensure the following steps are complete:
- MuJoCo is installed on your local device
- Docker is installed to run FEAGI
- Python environment is configured with the required dependencies for both FEAGI and MuJoCo.
- The FEAGI repository is cloned from [FEAGI GitHub](https://github.com/feagi/feagi)
- The `controller.py`file found [here](https://github.com/feagi/controllers/tree/feature-add-mujoco)

### Step 1: Setting up FEAGI
1. Clone the FEAGI repository and navigate to the project directory.
2. Use Docker to start FEAGI:
```plaintext
docker run -p 30000:30000 -p 4000:4000 feagi/feagi
```
3. Once running, FEAGI will listen on port 30000 and its Brain Visualizer will be available at http://127.0.0.1:4000/.

### Step 2: Running the Controller
1. Clone the controller repository and locate the controller.py file.
2. Run the controller script with the appropriate flag to specify the port:
```plaintext
python controller.py -port 30000
```
3. The controller will establish a connection with FEAGI and launch the MuJoCo viewer.

### Step 3: Visualizing Neural Activity
1. Open the Brain Visualizer at http://127.0.0.1:4000/.
2. If the visualizer does not load immediately, click on Genomes > Essential, and then restart the controller.
3. Observe the red dots representing neural activity. These dots dynamically change as the humanoid interacts with its simulated environment.

### Step 4: Controlling the Humanoid Manually
1. Within the Brain Visualizer, locate the 3D graph labeled Servo Position.
2. Shift + click on any box in the graph to select it. Selected boxes will turn green, and you can select multiple boxes at once.
3. Press the Space key to send data from FEAGI to MuJoCo. The corresponding joint and position values will cause the humanoid model to move, accounting for physics in the simulation.

### Troubleshooting Tips:
- If the Brain Visualizer fails to load, try restarting the controller script or refreshing the page.
- Ensure your firewall settings allow communication on ports 30000 and 4000.
- For macOS users, avoid running OpenGL and the MuJoCo viewer simultaneously to prevent threading conflicts.



## Challenges with Integration

Integrating the camera with Mujoco was challenging. The original plan was to capture RGB data from the camera and send it to FEAGI for processing. Mujoco includes built-in OpenGL rendering functions, which allowed us to retrieve the data. However, running OpenGL and the Mujoco viewer simultaneously is not possible on macOS due to threading issues.
To find a universal solution, we opted to use the rangefinder sensor included with Mujoco and attach it to the model. The rangefinder uses LiDAR technology, emitting a ray of light to measure the distance between its source and the first geom it intersects. We modified the Mujoco model to include multiple rays at varying angles with our source being the head. This collection of distance data can then be used to construct an image in FEAGI in place of RGB data.

![balloon image](https://drive.google.com/uc?id=17CqdGVeC765WS7RWPSsKupFyk19349P3)
## The Future

As mentioned in the introduction, the future awaits more adaptation to the humanoids genome, to gain as much control as possible. The team at Neuraville want to see the humanoid walk, run, dodge obstacles, etc. There’s no limit to what the humanoid model can do with the power of FEAGI, and we believe our work will be the foundation of lots of exploration to come. 

## Reflection

This was an incredible experience for our entire team. We got to work on a project we all were drawn to, and explore technology in a space that was brand new to us. It was a great opportunity to learn about the future of AI, and contribute to the best of our abilities in a really exciting space. We ran into a lot of challenges familiarizing ourselves with all these new tools, but the more we learned, the more impressed we grew with the capabilities of FEAGI in this new outlet. The team at Neuraville was amazing to work with, and provided us with endless support and constant communication over the term. We’re excited about the potential for future work and are grateful for the opportunity to contribute to this evolving field. On behalf of the team, thank you Neuraville. 



