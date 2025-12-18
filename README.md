This project is a from-scratch implementation of the LZW (Lempel–Ziv–Welch) compression algorithm in C, using a Trie-based dictionary for compression and a dynamic string table for decompression.

The goal of this project was to understand:

How real compression algorithms work internally

How dictionaries grow dynamically

How to manage variable-width codes (16 → 24 → 32 bits)

Memory management in C for non-trivial data structures

This is not a library — it’s an educational, low-level implementation.

Files
.
├── compress.c     // LZW compressor
├── decompress.c   // LZW decompressor
├── trie.c         // Trie implementation used by compressor
├── input.txt      // Input file for compression
├── compressed.dat // Output of compression
└── decompressed.dat // Output of decompression

How It Works (High Level)
Compression

Initializes a dictionary with all single-byte characters (0–255)

Uses a Trie to efficiently check if a sequence exists

Reads input byte-by-byte

Outputs dictionary codes instead of raw bytes

Automatically switches code size:

16-bit → 24-bit → 32-bit

Stores offsets in the output file header so decompression knows when code sizes change

Decompression

Reads header information (code-size offsets and max code)

Rebuilds the dictionary dynamically using a string table

Handles the special LZW case where a code is referenced before being fully defined

Reconstructs the original file exactly

Compilation

Use GCC:

gcc -O1 compress.c -o compress
gcc -O1 decompress.c -o decompress


trie.c is included directly in compress.c, so it does not need to be compiled separately.

Usage
Compress a file
./compress


Reads from input.txt

Writes compressed output to compressed.dat

Decompress the file
./decompress


Reads from compressed.dat

Writes reconstructed output to decompressed.dat

Notes & Limitations

Designed primarily for text and binary files

Dictionary growth is currently unbounded (up to 32-bit codes)

MAX_SEQUENCE_SIZE is defined but not yet enforced

Uses fgetc / fwrite for simplicity

Not optimized for speed or minimal memory usage

No error recovery for corrupted compressed files

Implementation Details

Trie-based dictionary for compression

Dynamic string table for decompression

Manual memory management (malloc, free)

Uses file offsets to track when code width changes

Portable C (no platform-specific APIs)

Why This Project?

This project was built to:

Go beyond “toy” data structures

Understand compression at the byte and bit level

Practice disciplined memory handling in C

Bridge theory (LZW) with real implementation details

Future Improvements

Enforce MAX_SEQUENCE_SIZE

Reset dictionary when full

Bit-level packing instead of byte-aligned codes

Streaming compression/decompression

Better error handling

Performance optimizations

Author

Written as a learning project by Mukhyansh.
