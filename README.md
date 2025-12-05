# fabric-ai-custom-pattern-generate_problems
Custom fabric-ai pattern to generate practice problems for general chemistry or differential equations

This pattern has served as an aid to my studying for university finals. Here is how to set it up, and most of all, use it properly.

# Set up
You'll find in this repository that there is (1) the "training" data I used in conjunction with `fabric-ai -p create_pattern` (a patttern to create more patterns) in order to refine the prompts, (2) the actual prompts as `system.md`. I've done it this way so you can see part of the process of how I create these patterns.

*Whenever I refer to 'data', I am referring to all the info I ultimately dump into `fabric-ai -p create_prompt` as the finale. I also have an alias set for `fabric-ai` as `fabric` (`echo 'alias fabric='fabric-ai'' > ~/.zshrc && source ~/.zshrc`)*

## This is how I set it up:
For the differential equations version, I used combinations of `pbpaste | fabric -p improve_prompt` (with the training data being the clipboard)

or `pbpaste | fabric -p clean_text | fabric -p summarize` whenever the data was messily pasted content.

Then the pattern was created with:
`cat training.txt | fabric -p clean_text | fabric -p create_pattern > system.md && open system.md`

I followed a very similar process for the chemistry version:
```
(fabric -p create_pattern < data.txt) > system.md
mkdir -p ~/.config/fabric/my-custom-patterns/chem_problems
cd ~/.config/fabric/my-custom-patterns/chem_problems
mv ~/Desktop/system.md ~/.config/fabric/my-custom-patterns/chem_problems
```

## Just like my previous repo (I will update and link it later, or you can find it), setting up the pattern is the exact same:
- Download the `system.md` for each pattern and put them in your custom patterns directory

- You can use LLMs run off your device, but cloud models from vendors such as `Cerebras` and `Ollama` are also great. I particularly like `Cerebras'` `qwen-3-235b-a22b-instruct-2507` or `Ollama's` `qwen3-vl:235b-cloud`. By doing so, you are opting in for faster speeds, but of course the concept is not as amazing as running the models on your own device.


# Usage
## diff_applications
`fabric -p diff_applications 'generate 35 problems' | fabric -p write_latex > sol.tex && pdflatex sol.tex && open -a preview sol.pdf`

The above command:
- use the custom pattern with additional input specifying how many problems to specify (this is from the pattern)
- use the output of the previous command as input for the `write_latex` command with `|`
- write the output onto a file called `sol.tex` with `>`, if it doesn't exist, it will be created. `.tex` is the file format for `LaTeX`. Having `LaTeX` installed will be useful for these patterns. Installation of `texlive` is for a future repo.
- Compile the `LaTeX` document with `pdflatex`, and `sol.pdf` is created.
- Viola! Open the pdf for your set of fresh practice problems.

## chem_problems
Usage will also be much handy with `LaTeX` just like the differential equations version:
`fabric -p chem_problems 'generate 55 problems focusing on chapters 3, 5, 7, and 9 concepts' | fabric -p write_latex 'do not write down any solutions' > ~/Desktop/sample.tex && cd ~/Desktop && pdflatex sample.tex && open sample.pdf`

The above command:
- Specify what concepts or chapters to focus the practice problems on, as well as how many. This was configured in the pattern. Use the `write_latex` and so on for a pdf output (much more readable)

# Notes
## I ran into 2 difficulties with the pre-made `write_latex` pattern.
1. My output was always including the solutions written as well! This defeats the entire purpose of these patterns. It is up to the user to solve the problems.
2. The `LaTeX` compilation step was always erroring out, seemingly because the model chose to use `LaTeX` packages not widely compatible (packages that were not installed along with `texlive`)

To fix this, I had to duplicate the pattern and alter it, saving it to my custom patterns instead. Adding the following lines fixed this issue. You may have to also include this workaround:
`echo 'you MUST only use latex from the following packages:' >> ~/.config/fabric/my-custom-patterns/write_latex/system.md`

and

`echo 'undero no circumstances should you ever write down the solutions to problems you generated, unless otherwise told to. always no matter what use widely compatible latex packages.' >> system.md`

<sub>side side note: these commands are under the assumption that you are working in the appropriate directories ***AND*** these fixes have already been implemented in the files within this repo. The above explanations are mainly for documentation and educational reasons.</sub>

### That ends the README, thank you for reading
I will be updating this project as I go as well.
