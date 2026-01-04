# Data & Analysis Code Card

## Data

COP Data: `COP_package_for_submission.csv`
TOI Data: `TOI_package_for_submission.csv`

### Columns & Description

Data is organized as a comma-separated-values (CSV) file with the following columns;

#### COP

- `torso_velocities` : Approach Torso Velocity measured using a 3D motion capture system.
- `toe_velocities` : Terminal Toe Velocity measured using a 3D motion capture system.
- `cop_hat` : Predicted COP.
- `time_gt` : Ground Truth TOI.
- `cop_gt` : Ground truth COP.
- `subject` : Subject identifier (using zero indexing).
 
#### TOI

- `torso_velocities` : Approach Torso Velocity measured using a 3D motion capture system.
- `toe_velocities` : Terminal Toe Velocity measured using a 3D motion capture system.
- `time_hat` : Predicted TOI.
- `time_gt` : Ground Truth TOI.
- `cop_gt` : Ground truth COP.
- `subject` : Subject identifier.


### Sample Analysis (Python)

#### Example for plotting a MAE vs FH scatter plot
##### COP
```
import pandas as pd
import matplotlib.pyplot as plt
from sklearn.metrics import mean_absolute_error

dir_to_cop_csv = "COP_package_for_submission.csv"

df = pd.read_csv(dir_to_cop_csv)
sub_df = df[df['subject'] == "sbj1"]

unique_times = sub_df.time_gt.unique()

MAEs = []
fhs = []

for time in unique_times:
    time_df = sub_df[sub_df['time_gt'] == time]
    preds = time_df.cop_hat
    gts = time_df.cop_gt

    MAEs.append(mean_absolute_error(preds,gts))
    fhs.append(time)

plt.clf()
plt.plot(fhs,MAEs)
plt.gca().invert_xaxis()
plt.xlabel("Forecast Horizon (ms)")
plt.ylabel("Mean-Absolute-Error (mm)")
```

##### TOI

```
import pandas as pd
import matplotlib.pyplot as plt
from sklearn.metrics import mean_absolute_error

dir_to_toi_csv = "TOI_package_for_submission.csv"

df = pd.read_csv(dir_to_toi_csv)
sub_df = df[df['subject'] == "sbj1"]

unique_times = sub_df.time_gt.unique()

MAEs = []
fhs = []

for time in unique_times:
    time_df = sub_df[sub_df['time_gt'] == time]
    preds = time_df.time_hat
    gts = time_df.time_gt

    MAEs.append(mean_absolute_error(preds,gts))
    fhs.append(time)

plt.clf()
plt.plot(fhs,MAEs)
plt.gca().invert_xaxis()
plt.xlabel("Forecast Horizon (ms)")
plt.ylabel("Mean-Absolute-Error (ms)")
```

