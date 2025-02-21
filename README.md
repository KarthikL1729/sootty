# Sootty

The process of designing hardware requires a graphical interaction mechanism, so that the designer can view the results of their hardware and understand how it functions on a low level. Currently, this is done using waveform viewers like Verdi and GTKWave, which are standalone programs that allow the designer to view and interact with the waveform. However, the process of interaction can be slow, inefficient, and difficult to automate. The goal of this project is to create a waveform viewer that can be used within the command line, enabling the designer to quickly view and debug hardware designs. Sootty is a terminal-based waveform viewer which takes advantage of modern terminals’ capabilities to give a full graphical display of the users’ waveforms.

<img width="979" alt="Screen Shot 2022-03-02 at 12 46 28 PM" src="https://user-images.githubusercontent.com/8484201/156447563-438b9763-5429-46f0-aa3f-7b5ad9fe6609.png">

Example of Sootty used in the terminal.

# How to use

This library can be installed from PyPi:

    python3 -m pip install sootty

To use, run:

    sootty waveform.vcd > image.svg

with a vcs file to produce an svg waveform diagram. Optional arguments include:
- `-s | --start FORMULA` Specity the start of the window.
- `-e | --end FORMULA` Specify the end of the window.
- `-l | --length N` Specify the number of ticks in the window (mutually exclusive with `-e`).
- `-d` Display the output to the terminal (requires viu).
- `-w | --wires LIST` Comma-separated list of wires to include in the visualization (default to all wires).

## Examples:

Display all wires starting at time 4 and ending at wire `clk`'s tenth tick:

    sootty example/example3.vcd -s "time 4" -e "acc clk == const 10" -w "clk,rst_n,core.pc,core.inst" -d

Display wires `Data` and `D1` for 8 units of time starting when `Data` is equal to 20:

    sootty example/example1.vcd -l 8 -s "Data == const 20" -w "D1,Data" -d

How to run in python (using the repl):

    from sootty import WireTrace, Visualizer, Style

    # Create wiretrace object from vcd file:
    wiretrace = WireTrace.from_vcd_file("example/example1.vcd")

    # Convert wiretrace to svg:
    image = Visualizer(Style.Dark).to_svg(wiretrace, start=0, length=8)
    
    # Display to stdout:
    image.display()

# Dependencies:

- viu (`git clone https://github.com/atanunq/viu.git viu/ && cd viu/ && cargo install --path .`)
- rsvg-convert (`brew install librsvg`)

# Contributing

If you are interested in contributing to this project, feel free to take a look at the existing issues. We also have a project idea listed for Google Summer of Code 2022 on the FOSSI website: https://www.fossi-foundation.org/gsoc22-ideas#enhancing-the-sootty-terminal-based-graphical-waveform-viewer
