# Sample Transcript

This is an example of what a lecture transcript might look like as input to the `/lecture` command.

## Expected Format

Transcripts can be:
- Plain text (`.txt`)
- Markdown (`.md`)
- With or without timestamps
- With or without speaker labels

The preprocessing step will clean:
- Timestamps like `[00:15:32]`
- Filler words ("um", "uh", "you know")
- Repeated words ("the the")
- Inaudible markers

## Example Input

```
Okay, hello everyone. Welcome to today's lecture on, um, machine learning.

Today we're going to talk about neural networks, yeah? And how they actually learn from data.

So the basic idea is that, you know, instead of programming rules manually, we let the computer discover patterns. Right?

[00:02:15]

And the way this works is through layers. Each layer extracts different features. The early layers find simple patterns like edges. The later layers combine them into more complex concepts.

Now, the key breakthrough was backpropagation. This algorithm, um, lets us efficiently compute how to update all the weights in the network. Without it, deep learning wouldn't be practical.

Let me show you on this slide...

[Shows diagram of neural network architecture]

As you can see, information flows forward through the network. Input comes in, gets transformed by each layer, and finally produces an output.

...
```

## Tips for Better Results

1. **Higher quality transcripts** produce better outputs
2. **Clean audio** → cleaner transcription → better processing
3. **Technical terms** should be correctly transcribed
4. **Speaker identification** helps with multi-speaker content

## Obtaining Transcripts

Options for creating transcripts:
- Whisper (OpenAI) - free, local, excellent quality
- Otter.ai - cloud service, good for meetings
- YouTube auto-captions - free, variable quality
- Professional transcription services
