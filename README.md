# Digital Processor Design (Latches, FSM, ALU, Decoder)

A simple processor built in Quartus from four integrated digital components: two 8-bit latches, a Moore-type FSM, a 4-to-16 decoder built from two 3-to-8 decoders, and a configurable ALU. Built for a Digital Systems course at Toronto Metropolitan University.

> **Note:** the original Quartus project files (`.qpf`/`.qsf`/HDL source) were built and submitted on a school lab computer and are not available for upload. This repo documents the design through the full lab report — block diagrams, waveforms, and truth tables for every stage.

## Overview

The processor takes two 8-bit operands (A and B, derived from the last four digits of a student ID) and stores them in latches. An FSM cycles through nine states (S0–S8), and each state is expanded by the 4-to-16 decoder into a 16-bit microcode word. That microcode selects which operation the ALU performs on A and B, with the result displayed in hexadecimal on seven-segment displays.

## Components

- **Latch (×2)** — 8-bit storage elements holding operands A and B. Sample on the rising clock edge; asynchronous reset clears output to `00000000`.
- **FSM** — Moore-type state machine, 9 states (S0–S8), outputs a 4-bit `current_state` to the decoder and a `student_id` value used for display.
- **4-to-16 Decoder** — built from two 3-to-8 decoders rather than a single hardcoded block, for speed. The MSB (S3) selects which 3-to-8 decoder is active; only one of 16 outputs asserts at a time, forming the ALU opcode.
- **ALU** — performs the operation selected by the microcode on operands A and B.

## Problem 1 — Baseline ALU (ALU1)

Implements the full pipeline (latches → FSM → decoder → ALU) with a baseline instruction set:

| Opcode | Operation |
|---|---|
| 0000 | sum(A, B) |
| 0001 | diff(A, B) |
| 0010 | NOT(A · B) |
| 0011 | NOT(A + B) |
| 0100 | A · B |
| 0101 | A ⊕ B |
| 0110 | A + B |
| 0111 | NOT(A ⊕ B) |

## Problem 2 — Extended ALU (ALU2)

Same FSM, latches, and decoder, with a new ALU implementing an assigned instruction set (Problem Set A):

| Opcode | Operation |
|---|---|
| 0000 | Increment A by 2 |
| 0001 | Shift B right by 2 bits, input bit = 0 |
| 0010 | Shift A right by 4 bits, input bit = 1 |
| 0011 | Min(A, B) |
| 0100 | Rotate A right by 2 bits (ROR) |
| 0101 | Invert the bit-significance order of B |
| 0110 | A ⊕ B |
| 0111 | sum(A, B) − 4 |
| 1000 | All output bits high |

## Problem 3 — Extended FSM (FSM2)

Same ALU (ALU1) and decoder as Problem 1, with a modified FSM adding a `student_id` and `id_sign` (even/odd) output per state, used to drive an additional seven-segment display.

## Verification

Each stage (latch, FSM, decoder, and each ALU/FSM variant) was verified independently via Quartus waveform simulation before integration, with truth tables confirming expected outputs at each clock cycle. All three problems produced correct microcode values and ALU results matching hand-derived expected outputs.

## Files

- `COE328_Lab6_Report.pdf` — full lab report: component descriptions, block diagrams, waveforms, truth tables, and problem set results for all three configurations
