.ONESHELL:
SHELL = bash


ifdef PBS_NP
PBS_NP := $(PBS_NP)
else
PBS_NP := 4
endif


.PHONY: clean
clean:
	rm -f \#*
	rm -f *.tpr *.trr *.edr *.log
	rm -f *_box* *_sol* system.*
	rm mdout.mdp

.PHONY: prepare
prepare:
	cp complex.top.template complex.top
	gmx editconf -f complex.gro -o complex_box.gro -d 0.8 -bt cubic
	gmx solvate -cp complex_box.gro -o complex_sol.gro -p complex.top
	gmx grompp -f em.mdp -c complex_sol.gro -p complex.top -o em.tpr -maxwarn 2
	gmx genion -s em.tpr -p complex.top -o system.gro -neutral -conc 0.15  << EOF
	15
	EOF
	rm em.tpr

.PHONY: opt
opt:
	gmx grompp -f em.mdp -c system.gro -p complex.top -o em.tpr -maxwarn 1
	gmx mdrun -v -deffnm em -nt $(PBS_NP) -nb cpu -pme cpu  -pmefft cpu -pin on

.PHONY: constraint
constraint:
	gmx grompp -f pr.mdp -c em.gro -p complex.top -r em.gro -o pr.tpr -maxwarn 1
	gmx mdrun -v  -deffnm pr -nt $(PBS_NP) -nb cpu -pme cpu  -pmefft cpu -pin on

.PHONY: build_index
build_index:
	gmx make_ndx -f pr.gro << EOF
	1|13
	!24
	name 24 protein_lig
	name 25 envir
	q
	EOF

.PHONY: md
md:
	gmx grompp -f md.mdp -c pr.gro -p complex.top -o md.tpr -n index.ndx -maxwarn 10
	gmx mdrun -v -deffnm md -nt $(PBS_NP) -ntmpi 1 -nb cpu -pme cpu  -pmefft cpu -pin on


.PHONY: gpu_md
gpu_md:
	gmx grompp -f md.mdp -c pr.gro -p complex.top -o md.tpr -n index.ndx -maxwarn 10
	gmx mdrun -v -deffnm md -nt $(PBS_NP) -ntmpi 1 -nb gpu -pme gpu  -pmefft gpu -pin on

.PHONY: traj
traj:
	gmx traj -f md.xtc -s md.tpr -oxt protein_lig.gro -n index.ndx -dt 100 << EOF
	24
	EOF
	pigz protein_lig.gro || gzip protein_lig.gro

.PHONY: traj_env
traj_env:
	gmx traj -f md.xtc -s md.tpr -oxt envir.gro -n index.ndx -dt 100 << EOF
	25
	EOF
	## compress for download
	pigz envir.gro || gzip envir.gro
