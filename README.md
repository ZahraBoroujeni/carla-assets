I have followed the page https://carla.readthedocs.io/en/latest/custom_assets_tutorials/ to create a dog assets. 
- The docker of carla to build assets is about 222GB, be sure you have enough space.
- When you `make package`, be sure that you put the assets in the exact folder path in the carla binary folder (it will be done automatically by ImportAssets.sh)
- I needed to change the scale and orientation of my .fbx file, so I have import the dog.fbx file inside the carla after running `make launch`, and then with Add/Import (Import Assets), I modified the scale and orientation.
- Then I test it by drag and drop the dog on a city map. Finally I close the carla and run `make package`. Copy the *.tar file inside Dist folder and unzip it. Find my Dog folder and copy to carla binary.

The Dog asset built for Carla simulation 0.9.15
Add Dog folder into the exact path Carla-0-9-15/CarlaUE4/Content/Carla/Static/Static/
And modify the file Carla-0-9-15/CarlaUE4/Content/Carla/Config/Default.Package.json and add the dog prop:
 ```json
       {
			"name": "Dog",
			"path": "/Game/Carla/Static/Static/Dog/source_dog_node_id126",
			"size": "Medium"
		},
```

test the props in Carla PythonAPI "static.prop.dog"
```py
import carla
import random
import time

client = carla.Client('localhost', 2000)
client.set_timeout(10.0)
world = client.get_world()

blueprints = world.get_blueprint_library()

# --- Find your prop blueprint ---
dog_bp = blueprints.find('static.prop.dog')   # <-- Your prop ID

# --- Set spawn transform ---
spawn_points = world.get_map().get_spawn_points()
spawn_transform = random.choice(spawn_points)

# --- Spawn as a static actor ---
dog_actor = world.spawn_actor(dog_bp, spawn_transform)

print("Spawned:", dog_actor)

```
