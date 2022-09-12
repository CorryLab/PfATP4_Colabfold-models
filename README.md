# PfATP4 structure as predicted by Colabfold

The purpose of these models was for docking of the anti-malarial compounds Cipargamin and SJ733, as 
part of the study "A G358S mutation in the Plasmodium falciparum Na+ pump PfATP4 confers clinically-relevant resistance to cipargamin" (Available in Nature Comms. soon)

In this study, two Colabfold structural predictions were utilised. Our own, which was produced using the default Colabfold parameters and is available in this repository.
The other was provided by the Ersilia Open Source Initiative, and is avaliable through [here](https://github.com/ersilia-os/osm-pfatp4-structure).




## Method
The wildtype pfatp4 structure was generated using the [Colabfold notebook](https://colab.research.google.com/github/sokrypton/ColabFold/blob/main/AlphaFold2.ipynb).
The UniprotKB sequence Q9U445 was trimmed of its first 115 residues and submitted to Colabfold for prediction of its structure. 
The structure with the highest plddt (pfatp4_86da6_unrelaxed_rank_1_model_3.pdb) then underwent further refinement
(raw json scores available in pfatp4_86da6_unrelaxed_rank_1_model_3_scores.json).

The structure was solvated with TIP3 water, the backbone atoms restrained, and energy minimised using the GROMACS 2021 Molecular dynamics software.
Below is the procedure utilised to energy minimise the predicted structure in Gromacs
```
# Amber99sb and tip3p water selected
gmx pdb2gmx -f test_86da6_unrelaxed_rank_1_model_3.pdb -o conf.gro

gmx editconf -f conf.gro -d 1 -o box.gro -princ 

gmx solvate -cp box.gro -p topol.top -o solv.gro

gmx make_ndx -f solv.gro

# Backbone atoms selected
gmx genrestr -f solv.gro -n index.ndx -o posre.itp

gmx grompp -f emin.mdp -c solv.gro -r solv.gro -p topol.top -n index.ndx -maxwarn 1
gmx mdrun -s topol.tpr -v -deffnm emin

# Waters removed and protein outputted
gmx editconf -f emin.gro -n index.ndx -o pfatp4-AF2_Colabfold.pdb
```

The G358S mutant structure (pfatp4-G358S_Colabfold.pdb) was generated by mutating pfatp4_Colabfold.pdb with the pdbreader tool on the [CHARMM-GUI website](https://www.charmm-gui.org/?doc=input/pdbreader). 




