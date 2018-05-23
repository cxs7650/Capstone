 “Completed Code in the Learning Environment”

import codecademylib
# Import both pandas and matplorlib modules
import pandas as pd
from matplotlib import pyplot as plt
# Create a DataFrame called species.
species= pd.read_csv('species_info.csv')
# Print DataFrame.
# Information provided: category, scientific_name,	common_names, &	conservation_status 
print(species.head())


# Calculating how many different species there are.
species_count= species.scientific_name.nunique()
print(species_count)

#Different values of category
species_type= species.category.unique()
print(species_type)

# Different values of conservation status
conservation_statuses = species.conservation_status.count()
print(conservation_statuses)

# Count how many scientific name falls into each criteria
conservation_counts= species.groupby('conservation_status').scientific_name.nunique().reset_index()
print(conservation_counts)

# Replace NaN with "No Intervention"
species.fillna('No Intervention', inplace = True)
# see how many species require No Intervention
conservation_counts_fixed= species.groupby('conservation_status').scientific_name.nunique().reset_index()
print(conservation_counts_fixed)

species = pd.read_csv('species_info.csv')

species.fillna('No Intervention', inplace = True)

protection_counts = species.groupby('conservation_status')\
    .scientific_name.nunique().reset_index()\
    .sort_values(by='scientific_name')
    
#Creating a Bar Chart
plt.figure(figsize=(10, 4))
ax = plt.subplot()
plt.bar(range(len(protection_counts)),protection_counts.scientific_name.values)

ax.set_xticks(range(len(protection_counts)))
ax.set_xticklabels(protection_counts.conservation_status.values)
plt.ylabel("Number of Species")
plt.title("Conservation Status by Species")

plt.show()


# creating a new column
species['is_protected'] = species.conservation_status.apply(lambda x: True if x != 'No Intervention' else False)

species.info()
# grouping by category and is protected
category_counts = species.groupby(['category', 'is_protected']).scientific_name.nunique().reset_index()

print(category_counts.head())

# pivoting the data
category_pivot= category_counts.pivot(columns = 'is_protected',
                              index= 'category',
                             	values= 'scientific_name').reset_index()
print(category_pivot)

# renaming the columns from True and False to protected and not protected
category_pivot.columns = ['category', 'not_protected', 'protected']
print(category_pivot)

# creating a new column called percent protected
category_pivot['percent_protected'] = category_pivot['protected'] / (category_pivot['not_protected'] + category_pivot['protected'])

print(category_pivot)




import codecademylib
import pandas as pd
from matplotlib import pyplot as plt

# Loading the Data
species = pd.read_csv('species_info.csv')

# print species.head()

# Inspecting the DataFrame
species_count = len(species)

species_type = species.category.unique()

conservation_statuses = species.conservation_status.unique()

# Analyze Species Conservation Status
conservation_counts = species.groupby('conservation_status').scientific_name.count().reset_index()

# print conservation_counts

# Analyze Species Conservation Status II
species.fillna('No Intervention', inplace = True)

conservation_counts_fixed = species.groupby('conservation_status').scientific_name.count().reset_index()

# Plotting Conservation Status by Species
protection_counts = species.groupby('conservation_status')\
    .scientific_name.count().reset_index()\
    .sort_values(by='scientific_name')
    
# plt.figure(figsize=(10, 4))
# ax = plt.subplot()
# plt.bar(range(len(protection_counts)),
#        protection_counts.scientific_name.values)
# ax.set_xticks(range(len(protection_counts)))
# ax.set_xticklabels(protection_counts.conservation_status.values)
# plt.ylabel('Number of Species')
# plt.title('Conservation Status by Species')
# labels = [e.get_text() for e in ax.get_xticklabels()]
# print ax.get_title()
# plt.show()

species['is_protected'] = species.conservation_status != 'No Intervention'

category_counts = species.groupby(['category', 'is_protected'])\
                         .scientific_name.count().reset_index()
  
# print category_counts.head()

category_pivot = category_counts.pivot(columns='is_protected', index='category', values='scientific_name').reset_index()

category_pivot.columns = ['category', 'not_protected', 'protected']

category_pivot['percent_protected'] = category_pivot.protected / (category_pivot.protected + category_pivot.not_protected)

print category_pivot.head()

# creating a table called contigency
contingency = [[30, 146], [75, 413]]

from scipy.stats import chi2_contingency

# run the chi2 test on contingency
chi2, pval, dof, expected = chi2_contingency(contingency)

print(pval)
# there is no significence since it is greater then 5% and that the null hypothesis is true
# testing the difference b/t reptile and mammal

contingency_2 = [[5, 73], [30, 146]]
chi2, pval_reptile_mammal, dof, expected = chi2_contingency(contingency_2)

print(pval_reptile_mammal)
# there is a difference since it less than 5% and the results are due to a random chance









import codecademylib
import pandas as pd
from matplotlib import pyplot as plt

species = pd.read_csv('species_info.csv')
species.fillna('No Intervention', inplace = True)
species['is_protected'] = species.conservation_status != 'No Intervention'

observations= pd.read_csv('observations.csv')
print(observations.head())

# creating a lambda function to create a new column in species
species['is_sheep'] = species.common_names.apply(lambda x: 'Sheep' in x)

species_is_sheep= species[species.is_sheep == True]

print(species_is_sheep.head())

sheep_species= species[(species.is_sheep ==True) &
                  		 (species.category == 'Mammal')]
print(sheep_species)

#merging sheep species with observation
sheep_observations = species.merge(sheep_species)\
											.merge(observations)
print(sheep_observations.head())

#total sheep sightings

obs_by_park = sheep_observations.groupby('park_name').observations.sum().reset_index()

print(obs_by_park)



import codecademylib
import pandas as pd
from matplotlib import pyplot as plt

species = pd.read_csv('species_info.csv')
species['is_sheep'] = species.common_names.apply(lambda x: 'Sheep' in x)
sheep_species = species[(species.is_sheep) & (species.category == 'Mammal')]

observations = pd.read_csv('observations.csv')

sheep_observations = observations.merge(sheep_species)

obs_by_park = sheep_observations.groupby('park_name').observations.sum().reset_index()

# create a bar chart
plt.figure(figsize=(16, 4))
ax = plt.subplot()
plt.bar(range(len(obs_by_park)),obs_by_park.observations.values)

ax.set_xticks(range(len(obs_by_park)))
ax.set_xticklabels(obs_by_park.park_name.values)
plt.ylabel("Number of Observations")
plt.title("Observations of Sheep per Week")

plt.show()




baseline= 15

minimum_detectable_effect= 100* 5/15.0 #33%
print(minimum_detectable_effect)

sample_size_per_variant= 890

yellowstone_weeks_observing = 890/507.0
print(yellowstone_weeks_observing)#1.75542406312
bryce_weeks_observing= 890/250.0
print(bryce_weeks_observing)#3.56









