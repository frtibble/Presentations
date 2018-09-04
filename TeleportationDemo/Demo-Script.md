# Quantum Teleportation Demo

## Step 1 - Data input
1. Open up `Driver.cs`
When writing a quantum program it's important to understand that not all of our code will be doing 'quantum' things. I'm going to walk you through what a quantum program looks like and explain the separations we have between quantum and classical code.

1. For anyone that's written any C sharp code, this should look familiar, because its a C sharp file. In our quantum program, we need what's called a 'Driver' which is classical code that drives our quantum code. What do I mean by that? Well if we look inside our Main function we've got this line here, where we create a 'QuantumSimulator' (line 14 of `Driver.cs`) - that's what we're going to use to run our program. We're using a simulator, but it could just as easily be real hardware. For example we can run Q sharp code on IBM's hardware, and naturally we'll be able to target Microsoft's hardware once it becomes available.
1. Now I want to provide my algorithm with some data, so I need to generate some. But first let me explain what that data looks like. I've described quantum teleportation as sending a state from one place to another. Now we're going to start off simply and just send a classical message, for example 'True' or 'False'. We can encode these as binary 1 or 0. So on this line (line 20 of `Driver.cs`) we randomly generate 'True' or 'False', and assign it to this 'sent' variable.

## Step 2 - Run algorithm (TeleportClassicalMessage)
1. Now we're going to run our quantum algorithm, which is going to 'TeleportClassicalMessage' which you can see here (line 21 of `Driver.cs`). This is a quantum operation, which uses the simulator we created, and the input data we created. But what does this operation look like? Since it's quantum, we define it in the Q sharp file.
1. Open 'Operation.qs'. Let's start by looking at our 'TeleportClassicalMessage' operation (line 88). It takes as input a boolean message, which we've just seen is provided in the C sharp file. So that's whether I want to teleport true or false. 
1. Inside the body of the operation we have this 'measurement' variable which is going to be our output, and I'll come back to that.
1. I explained earlier that for quantum teleportation we need 3 qubits, there's the message we wish to send, my qubit which we entangle with another qubit which is where the message is to be sent. So we're going to assign two of those qubits here, for the message (line 94) and where we're sending the message (line 95).
1. We pass those two qubits to a 'Teleport' operation which I'll come back to in a moment.
1. Then we're going to check if our teleportation was successful. So we're going to measure the qubit where we sent the message, and if the qubit state is 'One' we set our output measurement to be true, and if the qubit state is 'Zero' we set out output measurement to be false. That's just the boolean message encoding I was talking about.
1. Finally we reset our qubit register back to Zero as we've finished using them (line 108).
1. And then we return the result (line 111).

## Step 3 - Run algorithm (Teleport)
1. Now we get to the 'Teleport' operation (line 44). This takes a message qubit as input, and the destination qubit.
1. I said we needed 3 qubits and we've only got two, so we're going to allocate one more qubit for me (line 47). And assign that to a variable (line 50). So that's my qubit.
1. Now we're literally just going to follow the steps as I explained previously. First we needed to entangle my qubit with the destination qubit, which we did using a Hadamard gate, and a CNOT gate. You can see that with this H operation (line 53) and CNOT operation (line 54) here.
1. Next we wanted to move our message into the entangled pair. Which as I explained we did using CNOT and a Hadamard gate. In code that looks like this (line 57 and 58).
1. And finally, I performed two measurements, I measured my qubit, and the message qubit. That collapses those states down to two classical numbers, 0 or 1, and we used those to decide what operations we performed on the destination qubit. That was summarised in the table in this slide (switch back to slide as a reminder). Here, if this qubit is Zero we apply a Z each time, and if this qubit is Zero we apply an X each time. (Switch back to code) So you can see that with these two lines applying either Z or X.
1. And then once again, we reset the qubit we allocated. 

## Step 4 - Output
1. Now we're almost ready to run our program. The final thing we need to do is to process the output. So let's switch back to `Driver.cs`. On this line (line 21) you can see that we assign the output of our quantum code to this 'received' variable. The only processing we're going to do is to just print out that result to the console so we can see what it is. 
1. We're going to run this 8 times so let's have a look at the result. (Press start)

## Step 5 - BONUS
There's an additional example of teleporting a quantum message. This calls 'TeleportQuantumMessage' which prepares a qubit in the |+〉 ≔ H|0〉 state and asserts that the probability of obsering a Zero result is 50%. We then run it 1000 times and report back the average result (which should be roughly 50%). You can run this a few times to demonstrate that the number varies. 