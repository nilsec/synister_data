# Synister database export (v3)

## Contains
1. All annotated synapses in ``` synapses_v3.json ``` 
    Dictionary of synapse_ids to synapse attributes (skeleton_id, position, brain_region, splits)

2. All annotated skeletons in ``` skeletons_v3.json ```
    Dictionary of skeleton_ids to skeleton attributes (hemi_lineage_id, nt_known)

3. All annotated hemilineages in ```hemi_lineages_v3.json```  
    Dictionary of hemilineage_ids to hemilineage attributes (nt_guess, hemi_lineage_name)

4. All predictions for so far unknown neurotransmitters in ```predictions_fafb_v3_t8_p10.json``` 
    Dictionary of synapse_ids to our prediction and synapse attributes (prediction, position, brain_region, splits, hemi_lineage_id, nt_known).
    Predictions represent class probabilities in the following order:
    0:   gaba, 
    1:   acetylcholine, 
    2:   glutamate, 
    3:   dopamine, 
    4:   octopamine, 
    5:   serotonin 

Read files via:
```python
ìmport json

with open('predictions_fafb_v3_t8_p10.json') as f_p:
    predictions = json.load(f_p)
```

To get the predicted class of a synapse run:
```python
import numpy as np

synapse_class = np.argmax(predictions[synapse_id])
```

To get the predicted class of a skeleton run:
```python
# Collect all predictions in skeleton
classes_in_skeleton = []
for synapse_id, attributes in predictions.items():
	if attributes["skeleton_id"] == skeleton_id:
		classes_in_skeleton.append(np.argmax(attributes["prediction"]))	

# Majority vote:
skeleton_class = max(set(classes_in_skeleton), key=classes_in_skeleton.count)
```
