# The `CliffordSimp()` pass in TKET

The `CliffordSimp()` pass in `PyTKET` is an optimization pass that simplifies quantum circuits containing Clifford gates. Clifford gates are a particular class of quantum gates that are efficiently simulatable classically and can be implemented with very high fidelity in practice.


## Clifford gates

Clifford gates are a family of unitary gates in quantum computing that are particularly important because they are easy to simulate efficiently on classical computers. They are often used in the design of quantum algorithms, as well as in error correction codes.

The most common examples of Clifford gates are the Pauli gates (`X`, `Y`, and `Z`), the Hadamard gate (`H`), and the Controlled-NOT gate (`CNOT`). The single-qubit Clifford gates are those that can be obtained by composing rotations of the `X`, `Y`, and `Z` axes by multiples of 90 degrees, or by adding a global phase factor of either 1 or -1. The `CNOT` gate is also a Clifford gate, as it can be constructed from a sequence of single-qubit Clifford gates.

One important property of the Clifford gates is that they preserve the structure of the stabilizer group, which is a set of states that can be generated from a reference state by applying only Clifford gates. This property makes Clifford gates useful in the design of error correction codes, as errors that occur within the stabilizer group can be corrected using stabilizer measurements.The Clifford gates are also used in the design of some quantum algorithms, such as the quantum phase estimation algorithm and the quantum Fourier transform, which are used in a variety of applications, including factoring large integers and solving linear systems of equations.

### `TK1` gates

The `TK1` gate is a parameterized single-qubit gate that is not a Clifford gate, but can be expressed as a combination of the single-qubit Clifford gates and the `T` gate

The `TK1` gate is defined as follows:

```TK1(theta, phi, lambda) = Rz(phi) * Rx(theta) * Rz(lambda)```

where `Rz(phi)` and `Rx(theta)` are single-qubit rotation gates around the z and x axes, respectively, and `Rz(lambda)` is a single-qubit rotation gate around the z-axis.

We can express the `TK1` gate in terms of the more basic single-qubit Clifford gates (`H`, `S`) as follows:

```TK1(theta, phi, lambda) = H * Rz(theta + pi/2) * S * Rz(phi) * H * Rz(lambda - pi/2) * S```

Note that the `S` gate is equivalent to a pi/2 rotation around the z-axis.

## The `CliffordSimp()` pass: built-in simplification rules

The `CliffordSimp()` pass in PyTKET applies a series of simplification rules to a circuit containing Clifford gates, with the goal of producing an equivalent circuit that is more efficient and easier to execute on a quantum device. Some of the main simplification rules applied by the pass are:

1. Merge single-qubit Clifford gates: If two single-qubit Clifford gates of the same type are applied to the same qubit, they can be replaced by a single gate of the same type, possibly with a global phase factor.

2. Merge `CNOT` gates: If two or more `CNOT` gates with the same control and target qubits are applied in sequence, they can be merged into a single `CNOT` gate, possibly with some additional single-qubit gates.

3. Cancel adjacent gates: If two adjacent gates are inverses of each other, they can be removed from the circuit.

4. Simplify Z-phase gates: If a Z-phase gate is applied to a qubit that is the target of a `CNOT` gate, the Z-phase gate can be moved past the `CNOT` gate and replaced by an X-phase gate on the control qubit, possibly with some additional single-qubit gates.

5. Simplify Toffoli gates: If a Toffoli gate (controlled-controlled-NOT gate) is applied, it can be replaced by a sequence of `CNOT` gates and single-qubit Clifford gates.

These are just some of the simplification rules applied by the `CliffordSimp()` pass. The pass also includes many other rules for simplifying various types of Clifford gates, and can be customized to apply different sets of rules depending on the specific needs of the user.

The `CliffordSimp()` pass can be particularly useful when combined with other optimization passes that target non-Clifford gates. By first simplifying the Clifford gates in a circuit, the other optimization passes can then be more effective at simplifying the remaining non-Clifford gates.

The `CliffordSimp()` pass is part of a suite of optimization passes included in Pytket that can be used to improve the efficiency and fidelity of quantum circuits. Other passes include `FullPeepholeOptimise`, `DecomposeBoxes`, and `RemoveRedundancies`, among others.

### Built-in simplification rules examples

#### 1. Merge single-qubit Clifford gates

This examples shows how to merge single-qubit Clifford gates using PyTKET and the `CliffordSimp()` pass. We create a `Circuit` object with three single-qubit Clifford gates: an Ry gate on qubit 0, an H gate on qubit 0, and an Rz gate on qubit 1.


```python
from pytket.circuit import Circuit, OpType
from pytket.passes import CliffordSimp
from pytket.circuit.display import render_circuit_jupyter

# Create a circuit with some single-qubit Clifford gates
circ = Circuit(2)
circ.Ry(0.5, 0)
circ.H(0)
circ.Rz(0.25, 1)

# Render a circuit as jupyter cell output.
render_circuit_jupyter(circ)


```








<iframe srcdoc="
&lt;!DOCTYPE html&gt;
&lt;html lang=&#34;en&#34;&gt;
&lt;head&gt;
    &lt;meta charset=&#34;UTF-8&#34;&gt;
    &lt;script type=&#34;application/javascript&#34; src=&#34;https://cdn.jsdelivr.net/npm/vue@3&#34;&gt;&lt;/script&gt;
    &lt;script type=&#34;application/javascript&#34; src=&#34;https://unpkg.com/pytket-circuit-renderer@0.3/dist/pytket-circuit-renderer.umd.js&#34;&gt;&lt;/script&gt;
    &lt;link rel=&#34;stylesheet&#34; href=&#34;https://unpkg.com/pytket-circuit-renderer@0.3/dist/pytket-circuit-renderer.css&#34;&gt;
&lt;/head&gt;
&lt;body&gt;
    &lt;div id=&#34;circuit-display-vue-container-d62d9c50-b575-4cd9-8069-ad4b870d7a0e&#34; class=&#34;pytket-circuit-display-container&#34;&gt;
        &lt;div style=&#34;display: none&#34;&gt;
            &lt;div id=&#34;circuit-json-to-display&#34;&gt;{&#34;bits&#34;: [], &#34;commands&#34;: [{&#34;args&#34;: [[&#34;q&#34;, [0]]], &#34;op&#34;: {&#34;params&#34;: [&#34;0.5&#34;], &#34;type&#34;: &#34;Ry&#34;}}, {&#34;args&#34;: [[&#34;q&#34;, [1]]], &#34;op&#34;: {&#34;params&#34;: [&#34;0.25&#34;], &#34;type&#34;: &#34;Rz&#34;}}, {&#34;args&#34;: [[&#34;q&#34;, [0]]], &#34;op&#34;: {&#34;type&#34;: &#34;H&#34;}}], &#34;created_qubits&#34;: [], &#34;discarded_qubits&#34;: [], &#34;implicit_permutation&#34;: [[[&#34;q&#34;, [0]], [&#34;q&#34;, [0]]], [[&#34;q&#34;, [1]], [&#34;q&#34;, [1]]]], &#34;phase&#34;: &#34;0.0&#34;, &#34;qubits&#34;: [[&#34;q&#34;, [0]], [&#34;q&#34;, [1]]]}&lt;/div&gt;
        &lt;/div&gt;
        &lt;circuit-display-container :circuit-element-str=&#34;&#39;#circuit-json-to-display&#39;&#34;&gt;&lt;/circuit-display-container&gt;
    &lt;/div&gt;
    &lt;script type=&#34;application/javascript&#34;&gt;
        const { createApp } = Vue;
        const circuitDisplayContainer = window[&#34;pytket-circuit-renderer&#34;].default;
        // Init variables to be shared between circuit display instances
        if (typeof window.pytketCircuitDisplays === &#34;undefined&#34;) {
            window.pytketCircuitDisplays = {};
        }
        const uid = &#34;d62d9c50-b575-4cd9-8069-ad4b870d7a0e&#34;;
        // Create the root Vue component
        const app = createApp({
            delimiters: [&#39;[[#&#39;, &#39;#]]&#39;],
            components: { circuitDisplayContainer },
        })
        app.config.unwrapInjectedRef = true;
        app.mount(&#34;#circuit-display-vue-container-&#34;+uid);
        window.pytketCircuitDisplays[uid] = app;
    &lt;/script&gt;
&lt;/body&gt;
&lt;/html&gt;
"
        width="100%" height="200px"
        style="border: none; outline: none; resize: vertical; overflow: auto"></iframe>




We then apply the `CliffordSimp()` pass to the circuit, which will merge any adjacent single-qubit Clifford gates into a single gate, if possible. Finally, we print the resulting circuit to see the simplification.


```python
# Apply the CliffordSimp pass to merge the gates
CliffordSimp().apply(circ)

# Render a circuit as jupyter cell output.
render_circuit_jupyter(circ)
```








<iframe srcdoc="
&lt;!DOCTYPE html&gt;
&lt;html lang=&#34;en&#34;&gt;
&lt;head&gt;
    &lt;meta charset=&#34;UTF-8&#34;&gt;
    &lt;script type=&#34;application/javascript&#34; src=&#34;https://cdn.jsdelivr.net/npm/vue@3&#34;&gt;&lt;/script&gt;
    &lt;script type=&#34;application/javascript&#34; src=&#34;https://unpkg.com/pytket-circuit-renderer@0.3/dist/pytket-circuit-renderer.umd.js&#34;&gt;&lt;/script&gt;
    &lt;link rel=&#34;stylesheet&#34; href=&#34;https://unpkg.com/pytket-circuit-renderer@0.3/dist/pytket-circuit-renderer.css&#34;&gt;
&lt;/head&gt;
&lt;body&gt;
    &lt;div id=&#34;circuit-display-vue-container-a6599473-88f0-4379-8d37-25f9a69f2280&#34; class=&#34;pytket-circuit-display-container&#34;&gt;
        &lt;div style=&#34;display: none&#34;&gt;
            &lt;div id=&#34;circuit-json-to-display&#34;&gt;{&#34;bits&#34;: [], &#34;commands&#34;: [{&#34;args&#34;: [[&#34;q&#34;, [0]]], &#34;op&#34;: {&#34;params&#34;: [&#34;0&#34;, &#34;0&#34;, &#34;1&#34;], &#34;type&#34;: &#34;TK1&#34;}}, {&#34;args&#34;: [[&#34;q&#34;, [1]]], &#34;op&#34;: {&#34;params&#34;: [&#34;0&#34;, &#34;0&#34;, &#34;0.25&#34;], &#34;type&#34;: &#34;TK1&#34;}}], &#34;created_qubits&#34;: [], &#34;discarded_qubits&#34;: [], &#34;implicit_permutation&#34;: [[[&#34;q&#34;, [0]], [&#34;q&#34;, [0]]], [[&#34;q&#34;, [1]], [&#34;q&#34;, [1]]]], &#34;phase&#34;: &#34;0.5&#34;, &#34;qubits&#34;: [[&#34;q&#34;, [0]], [&#34;q&#34;, [1]]]}&lt;/div&gt;
        &lt;/div&gt;
        &lt;circuit-display-container :circuit-element-str=&#34;&#39;#circuit-json-to-display&#39;&#34;&gt;&lt;/circuit-display-container&gt;
    &lt;/div&gt;
    &lt;script type=&#34;application/javascript&#34;&gt;
        const { createApp } = Vue;
        const circuitDisplayContainer = window[&#34;pytket-circuit-renderer&#34;].default;
        // Init variables to be shared between circuit display instances
        if (typeof window.pytketCircuitDisplays === &#34;undefined&#34;) {
            window.pytketCircuitDisplays = {};
        }
        const uid = &#34;a6599473-88f0-4379-8d37-25f9a69f2280&#34;;
        // Create the root Vue component
        const app = createApp({
            delimiters: [&#39;[[#&#39;, &#39;#]]&#39;],
            components: { circuitDisplayContainer },
        })
        app.config.unwrapInjectedRef = true;
        app.mount(&#34;#circuit-display-vue-container-&#34;+uid);
        window.pytketCircuitDisplays[uid] = app;
    &lt;/script&gt;
&lt;/body&gt;
&lt;/html&gt;
"
        width="100%" height="200px"
        style="border: none; outline: none; resize: vertical; overflow: auto"></iframe>




We see that the `CliffordSimp()` pass gives `TK1` gates. The `TK1` gate is a generalization of the single-qubit Clifford gates that allows arbitrary single-qubit rotations around the Z, X, and Y axes. The `CliffordSimp()` pass in PyTKET will attempt to merge `TK1` gates along with the standard Clifford gates, but it may not always succeed in doing so.

If you prefer to use the standard Clifford gates `Rx`, `Ry`, and `Rz` instead of the `TK1` gate, you can add the `auto_rebase_pass()` pass to your optimization pipeline to convert `TK1` gates to standard Clifford gates. 

<div class="alert alert-success">
<strong>Note:<strong> The rebase pass needs to know how to handle 2-qubit gates as well as 1-qubit gates. So our target gateset needs to include at least one 2-qubit gate.
</div>

Here's an example of how to do this:


```python
from pytket.passes import auto_rebase_pass

# Create the AutoRebase pass with the desired gate set
rebase_pass = auto_rebase_pass({OpType.Rz, OpType.Rx, OpType.Ry, OpType.CX})

# Apply the DecomposeTK1 pass to convert the TK1 gates to Rx, Ry, Rz
rebase_pass.apply(circ)

# Render a circuit as jupyter cell output.
render_circuit_jupyter(circ)
```








<iframe srcdoc="
&lt;!DOCTYPE html&gt;
&lt;html lang=&#34;en&#34;&gt;
&lt;head&gt;
    &lt;meta charset=&#34;UTF-8&#34;&gt;
    &lt;script type=&#34;application/javascript&#34; src=&#34;https://cdn.jsdelivr.net/npm/vue@3&#34;&gt;&lt;/script&gt;
    &lt;script type=&#34;application/javascript&#34; src=&#34;https://unpkg.com/pytket-circuit-renderer@0.3/dist/pytket-circuit-renderer.umd.js&#34;&gt;&lt;/script&gt;
    &lt;link rel=&#34;stylesheet&#34; href=&#34;https://unpkg.com/pytket-circuit-renderer@0.3/dist/pytket-circuit-renderer.css&#34;&gt;
&lt;/head&gt;
&lt;body&gt;
    &lt;div id=&#34;circuit-display-vue-container-a2f99926-bc41-4ddb-9307-53d33c9a759f&#34; class=&#34;pytket-circuit-display-container&#34;&gt;
        &lt;div style=&#34;display: none&#34;&gt;
            &lt;div id=&#34;circuit-json-to-display&#34;&gt;{&#34;bits&#34;: [], &#34;commands&#34;: [{&#34;args&#34;: [[&#34;q&#34;, [0]]], &#34;op&#34;: {&#34;params&#34;: [&#34;1&#34;], &#34;type&#34;: &#34;Rz&#34;}}, {&#34;args&#34;: [[&#34;q&#34;, [1]]], &#34;op&#34;: {&#34;params&#34;: [&#34;0.25&#34;], &#34;type&#34;: &#34;Rz&#34;}}], &#34;created_qubits&#34;: [], &#34;discarded_qubits&#34;: [], &#34;implicit_permutation&#34;: [[[&#34;q&#34;, [0]], [&#34;q&#34;, [0]]], [[&#34;q&#34;, [1]], [&#34;q&#34;, [1]]]], &#34;phase&#34;: &#34;0.5&#34;, &#34;qubits&#34;: [[&#34;q&#34;, [0]], [&#34;q&#34;, [1]]]}&lt;/div&gt;
        &lt;/div&gt;
        &lt;circuit-display-container :circuit-element-str=&#34;&#39;#circuit-json-to-display&#39;&#34;&gt;&lt;/circuit-display-container&gt;
    &lt;/div&gt;
    &lt;script type=&#34;application/javascript&#34;&gt;
        const { createApp } = Vue;
        const circuitDisplayContainer = window[&#34;pytket-circuit-renderer&#34;].default;
        // Init variables to be shared between circuit display instances
        if (typeof window.pytketCircuitDisplays === &#34;undefined&#34;) {
            window.pytketCircuitDisplays = {};
        }
        const uid = &#34;a2f99926-bc41-4ddb-9307-53d33c9a759f&#34;;
        // Create the root Vue component
        const app = createApp({
            delimiters: [&#39;[[#&#39;, &#39;#]]&#39;],
            components: { circuitDisplayContainer },
        })
        app.config.unwrapInjectedRef = true;
        app.mount(&#34;#circuit-display-vue-container-&#34;+uid);
        window.pytketCircuitDisplays[uid] = app;
    &lt;/script&gt;
&lt;/body&gt;
&lt;/html&gt;
"
        width="100%" height="200px"
        style="border: none; outline: none; resize: vertical; overflow: auto"></iframe>




So we see that in this example, the `CliffordSimp()` pass will identify that the `Ry` and `H` gates can be merged to form a single `Rz` gate (or `TK1` gate), resulting in a simplified circuit with two gates.

#### 2. Merge CNOT gates:

In this example, create a `Circuit` object with some Clifford gates: three CNOT gates.


```python
from pytket import Circuit
from pytket.passes import CliffordSimp
from pytket.circuit.display import render_circuit_jupyter

# Create a circuit with some Clifford gates
circ = Circuit(3)
circ.CX(1, 0).CX(1, 2).CX(1, 0)

# Render a circuit as jupyter cell output.
render_circuit_jupyter(circ)
```








<iframe srcdoc="
&lt;!DOCTYPE html&gt;
&lt;html lang=&#34;en&#34;&gt;
&lt;head&gt;
    &lt;meta charset=&#34;UTF-8&#34;&gt;
    &lt;script type=&#34;application/javascript&#34; src=&#34;https://cdn.jsdelivr.net/npm/vue@3&#34;&gt;&lt;/script&gt;
    &lt;script type=&#34;application/javascript&#34; src=&#34;https://unpkg.com/pytket-circuit-renderer@0.3/dist/pytket-circuit-renderer.umd.js&#34;&gt;&lt;/script&gt;
    &lt;link rel=&#34;stylesheet&#34; href=&#34;https://unpkg.com/pytket-circuit-renderer@0.3/dist/pytket-circuit-renderer.css&#34;&gt;
&lt;/head&gt;
&lt;body&gt;
    &lt;div id=&#34;circuit-display-vue-container-0234948b-b23a-4fb8-8612-8c4efaa82c10&#34; class=&#34;pytket-circuit-display-container&#34;&gt;
        &lt;div style=&#34;display: none&#34;&gt;
            &lt;div id=&#34;circuit-json-to-display&#34;&gt;{&#34;bits&#34;: [], &#34;commands&#34;: [{&#34;args&#34;: [[&#34;q&#34;, [1]], [&#34;q&#34;, [0]]], &#34;op&#34;: {&#34;type&#34;: &#34;CX&#34;}}, {&#34;args&#34;: [[&#34;q&#34;, [1]], [&#34;q&#34;, [2]]], &#34;op&#34;: {&#34;type&#34;: &#34;CX&#34;}}, {&#34;args&#34;: [[&#34;q&#34;, [1]], [&#34;q&#34;, [0]]], &#34;op&#34;: {&#34;type&#34;: &#34;CX&#34;}}], &#34;created_qubits&#34;: [], &#34;discarded_qubits&#34;: [], &#34;implicit_permutation&#34;: [[[&#34;q&#34;, [0]], [&#34;q&#34;, [0]]], [[&#34;q&#34;, [1]], [&#34;q&#34;, [1]]], [[&#34;q&#34;, [2]], [&#34;q&#34;, [2]]]], &#34;phase&#34;: &#34;0.0&#34;, &#34;qubits&#34;: [[&#34;q&#34;, [0]], [&#34;q&#34;, [1]], [&#34;q&#34;, [2]]]}&lt;/div&gt;
        &lt;/div&gt;
        &lt;circuit-display-container :circuit-element-str=&#34;&#39;#circuit-json-to-display&#39;&#34;&gt;&lt;/circuit-display-container&gt;
    &lt;/div&gt;
    &lt;script type=&#34;application/javascript&#34;&gt;
        const { createApp } = Vue;
        const circuitDisplayContainer = window[&#34;pytket-circuit-renderer&#34;].default;
        // Init variables to be shared between circuit display instances
        if (typeof window.pytketCircuitDisplays === &#34;undefined&#34;) {
            window.pytketCircuitDisplays = {};
        }
        const uid = &#34;0234948b-b23a-4fb8-8612-8c4efaa82c10&#34;;
        // Create the root Vue component
        const app = createApp({
            delimiters: [&#39;[[#&#39;, &#39;#]]&#39;],
            components: { circuitDisplayContainer },
        })
        app.config.unwrapInjectedRef = true;
        app.mount(&#34;#circuit-display-vue-container-&#34;+uid);
        window.pytketCircuitDisplays[uid] = app;
    &lt;/script&gt;
&lt;/body&gt;
&lt;/html&gt;
"
        width="100%" height="200px"
        style="border: none; outline: none; resize: vertical; overflow: auto"></iframe>




We then create a `CliffordSimp()` pass object and apply it to the circuit using the apply method. This simplifies the circuit by replacing the CNOT and Hadamard gates with equivalent sequences of simpler Clifford gates.


```python
# Apply the CliffordSimp pass to the circuit
CliffordSimp().apply(circ)
```




    True



Finally, we display the resulting optimized circuit. The output should show the original circuit with the Clifford gates replaced by simpler equivalent. 


```python
#Render a circuit as jupyter cell output.
render_circuit_jupyter(circ)
```








<iframe srcdoc="
&lt;!DOCTYPE html&gt;
&lt;html lang=&#34;en&#34;&gt;
&lt;head&gt;
    &lt;meta charset=&#34;UTF-8&#34;&gt;
    &lt;script type=&#34;application/javascript&#34; src=&#34;https://cdn.jsdelivr.net/npm/vue@3&#34;&gt;&lt;/script&gt;
    &lt;script type=&#34;application/javascript&#34; src=&#34;https://unpkg.com/pytket-circuit-renderer@0.3/dist/pytket-circuit-renderer.umd.js&#34;&gt;&lt;/script&gt;
    &lt;link rel=&#34;stylesheet&#34; href=&#34;https://unpkg.com/pytket-circuit-renderer@0.3/dist/pytket-circuit-renderer.css&#34;&gt;
&lt;/head&gt;
&lt;body&gt;
    &lt;div id=&#34;circuit-display-vue-container-72fa9b86-eb6d-4b7b-84ee-e8835666186d&#34; class=&#34;pytket-circuit-display-container&#34;&gt;
        &lt;div style=&#34;display: none&#34;&gt;
            &lt;div id=&#34;circuit-json-to-display&#34;&gt;{&#34;bits&#34;: [], &#34;commands&#34;: [{&#34;args&#34;: [[&#34;q&#34;, [1]], [&#34;q&#34;, [2]]], &#34;op&#34;: {&#34;type&#34;: &#34;CX&#34;}}], &#34;created_qubits&#34;: [], &#34;discarded_qubits&#34;: [], &#34;implicit_permutation&#34;: [[[&#34;q&#34;, [0]], [&#34;q&#34;, [0]]], [[&#34;q&#34;, [1]], [&#34;q&#34;, [1]]], [[&#34;q&#34;, [2]], [&#34;q&#34;, [2]]]], &#34;phase&#34;: &#34;0.0&#34;, &#34;qubits&#34;: [[&#34;q&#34;, [0]], [&#34;q&#34;, [1]], [&#34;q&#34;, [2]]]}&lt;/div&gt;
        &lt;/div&gt;
        &lt;circuit-display-container :circuit-element-str=&#34;&#39;#circuit-json-to-display&#39;&#34;&gt;&lt;/circuit-display-container&gt;
    &lt;/div&gt;
    &lt;script type=&#34;application/javascript&#34;&gt;
        const { createApp } = Vue;
        const circuitDisplayContainer = window[&#34;pytket-circuit-renderer&#34;].default;
        // Init variables to be shared between circuit display instances
        if (typeof window.pytketCircuitDisplays === &#34;undefined&#34;) {
            window.pytketCircuitDisplays = {};
        }
        const uid = &#34;72fa9b86-eb6d-4b7b-84ee-e8835666186d&#34;;
        // Create the root Vue component
        const app = createApp({
            delimiters: [&#39;[[#&#39;, &#39;#]]&#39;],
            components: { circuitDisplayContainer },
        })
        app.config.unwrapInjectedRef = true;
        app.mount(&#34;#circuit-display-vue-container-&#34;+uid);
        window.pytketCircuitDisplays[uid] = app;
    &lt;/script&gt;
&lt;/body&gt;
&lt;/html&gt;
"
        width="100%" height="200px"
        style="border: none; outline: none; resize: vertical; overflow: auto"></iframe>




#### 3. Cancel adjacent gates:

In this example, the `CliffordSimp()` pass is used to cancel adjacent gates:


```python
import pytket
from pytket.passes import CliffordSimp

# Create a simple 3-qubit circuit with adjacent X and H gates
circuit = pytket.Circuit(3)
circuit.H(0)
circuit.X(0)
circuit.H(1)
circuit.X(1)

#Render a circuit as jupyter cell output.
render_circuit_jupyter(circuit)

# Apply the CliffordSimp pass to the circuit to cancel adjacent gates
CliffordSimp().apply(circuit)

#Render a circuit as jupyter cell output.
render_circuit_jupyter(circuit)

```








<iframe srcdoc="
&lt;!DOCTYPE html&gt;
&lt;html lang=&#34;en&#34;&gt;
&lt;head&gt;
    &lt;meta charset=&#34;UTF-8&#34;&gt;
    &lt;script type=&#34;application/javascript&#34; src=&#34;https://cdn.jsdelivr.net/npm/vue@3&#34;&gt;&lt;/script&gt;
    &lt;script type=&#34;application/javascript&#34; src=&#34;https://unpkg.com/pytket-circuit-renderer@0.3/dist/pytket-circuit-renderer.umd.js&#34;&gt;&lt;/script&gt;
    &lt;link rel=&#34;stylesheet&#34; href=&#34;https://unpkg.com/pytket-circuit-renderer@0.3/dist/pytket-circuit-renderer.css&#34;&gt;
&lt;/head&gt;
&lt;body&gt;
    &lt;div id=&#34;circuit-display-vue-container-7486df01-eb4f-4759-9f45-7b3c61ff4783&#34; class=&#34;pytket-circuit-display-container&#34;&gt;
        &lt;div style=&#34;display: none&#34;&gt;
            &lt;div id=&#34;circuit-json-to-display&#34;&gt;{&#34;bits&#34;: [], &#34;commands&#34;: [{&#34;args&#34;: [[&#34;q&#34;, [0]]], &#34;op&#34;: {&#34;type&#34;: &#34;H&#34;}}, {&#34;args&#34;: [[&#34;q&#34;, [1]]], &#34;op&#34;: {&#34;type&#34;: &#34;H&#34;}}, {&#34;args&#34;: [[&#34;q&#34;, [0]]], &#34;op&#34;: {&#34;type&#34;: &#34;X&#34;}}, {&#34;args&#34;: [[&#34;q&#34;, [1]]], &#34;op&#34;: {&#34;type&#34;: &#34;X&#34;}}], &#34;created_qubits&#34;: [], &#34;discarded_qubits&#34;: [], &#34;implicit_permutation&#34;: [[[&#34;q&#34;, [0]], [&#34;q&#34;, [0]]], [[&#34;q&#34;, [1]], [&#34;q&#34;, [1]]], [[&#34;q&#34;, [2]], [&#34;q&#34;, [2]]]], &#34;phase&#34;: &#34;0.0&#34;, &#34;qubits&#34;: [[&#34;q&#34;, [0]], [&#34;q&#34;, [1]], [&#34;q&#34;, [2]]]}&lt;/div&gt;
        &lt;/div&gt;
        &lt;circuit-display-container :circuit-element-str=&#34;&#39;#circuit-json-to-display&#39;&#34;&gt;&lt;/circuit-display-container&gt;
    &lt;/div&gt;
    &lt;script type=&#34;application/javascript&#34;&gt;
        const { createApp } = Vue;
        const circuitDisplayContainer = window[&#34;pytket-circuit-renderer&#34;].default;
        // Init variables to be shared between circuit display instances
        if (typeof window.pytketCircuitDisplays === &#34;undefined&#34;) {
            window.pytketCircuitDisplays = {};
        }
        const uid = &#34;7486df01-eb4f-4759-9f45-7b3c61ff4783&#34;;
        // Create the root Vue component
        const app = createApp({
            delimiters: [&#39;[[#&#39;, &#39;#]]&#39;],
            components: { circuitDisplayContainer },
        })
        app.config.unwrapInjectedRef = true;
        app.mount(&#34;#circuit-display-vue-container-&#34;+uid);
        window.pytketCircuitDisplays[uid] = app;
    &lt;/script&gt;
&lt;/body&gt;
&lt;/html&gt;
"
        width="100%" height="200px"
        style="border: none; outline: none; resize: vertical; overflow: auto"></iframe>











<iframe srcdoc="
&lt;!DOCTYPE html&gt;
&lt;html lang=&#34;en&#34;&gt;
&lt;head&gt;
    &lt;meta charset=&#34;UTF-8&#34;&gt;
    &lt;script type=&#34;application/javascript&#34; src=&#34;https://cdn.jsdelivr.net/npm/vue@3&#34;&gt;&lt;/script&gt;
    &lt;script type=&#34;application/javascript&#34; src=&#34;https://unpkg.com/pytket-circuit-renderer@0.3/dist/pytket-circuit-renderer.umd.js&#34;&gt;&lt;/script&gt;
    &lt;link rel=&#34;stylesheet&#34; href=&#34;https://unpkg.com/pytket-circuit-renderer@0.3/dist/pytket-circuit-renderer.css&#34;&gt;
&lt;/head&gt;
&lt;body&gt;
    &lt;div id=&#34;circuit-display-vue-container-2932d868-5e88-4e96-87fb-3f2ac6086bab&#34; class=&#34;pytket-circuit-display-container&#34;&gt;
        &lt;div style=&#34;display: none&#34;&gt;
            &lt;div id=&#34;circuit-json-to-display&#34;&gt;{&#34;bits&#34;: [], &#34;commands&#34;: [{&#34;args&#34;: [[&#34;q&#34;, [0]]], &#34;op&#34;: {&#34;params&#34;: [&#34;1/2&#34;, &#34;1/2&#34;, &#34;-1/2&#34;], &#34;type&#34;: &#34;TK1&#34;}}, {&#34;args&#34;: [[&#34;q&#34;, [1]]], &#34;op&#34;: {&#34;params&#34;: [&#34;1/2&#34;, &#34;1/2&#34;, &#34;-1/2&#34;], &#34;type&#34;: &#34;TK1&#34;}}], &#34;created_qubits&#34;: [], &#34;discarded_qubits&#34;: [], &#34;implicit_permutation&#34;: [[[&#34;q&#34;, [0]], [&#34;q&#34;, [0]]], [[&#34;q&#34;, [1]], [&#34;q&#34;, [1]]], [[&#34;q&#34;, [2]], [&#34;q&#34;, [2]]]], &#34;phase&#34;: &#34;0.0&#34;, &#34;qubits&#34;: [[&#34;q&#34;, [0]], [&#34;q&#34;, [1]], [&#34;q&#34;, [2]]]}&lt;/div&gt;
        &lt;/div&gt;
        &lt;circuit-display-container :circuit-element-str=&#34;&#39;#circuit-json-to-display&#39;&#34;&gt;&lt;/circuit-display-container&gt;
    &lt;/div&gt;
    &lt;script type=&#34;application/javascript&#34;&gt;
        const { createApp } = Vue;
        const circuitDisplayContainer = window[&#34;pytket-circuit-renderer&#34;].default;
        // Init variables to be shared between circuit display instances
        if (typeof window.pytketCircuitDisplays === &#34;undefined&#34;) {
            window.pytketCircuitDisplays = {};
        }
        const uid = &#34;2932d868-5e88-4e96-87fb-3f2ac6086bab&#34;;
        // Create the root Vue component
        const app = createApp({
            delimiters: [&#39;[[#&#39;, &#39;#]]&#39;],
            components: { circuitDisplayContainer },
        })
        app.config.unwrapInjectedRef = true;
        app.mount(&#34;#circuit-display-vue-container-&#34;+uid);
        window.pytketCircuitDisplays[uid] = app;
    &lt;/script&gt;
&lt;/body&gt;
&lt;/html&gt;
"
        width="100%" height="200px"
        style="border: none; outline: none; resize: vertical; overflow: auto"></iframe>




In this example, the `CliffordSimp()` pass is applied to a circuit containing adjacent `H` and `X` gates on qubits 0 and 1. The pass simplifies the circuit by cancelling these adjacent gates, resulting in a simpler circuit with only the necessary gates. 

#### 4. Simplify Z-phase gates:

Now we use the `CliffordSimp()` pass in PyTKET to simplify circuits with Z-phase gates:


```python
from pytket.circuit import Circuit, Qubit
from pytket.passes import CliffordSimp

# Create a circuit with some Z-phase gates
c = Circuit(3)
c.Z(0)
c.Rz(0.25, 1)
c.Z(1)
c.Z(1)

#Render a circuit as jupyter cell output.
render_circuit_jupyter(c)

# Apply the CliffordSimp pass to simplify the circuit
CliffordSimp().apply(c)

#Render a circuit as jupyter cell output.
render_circuit_jupyter(c)

```








<iframe srcdoc="
&lt;!DOCTYPE html&gt;
&lt;html lang=&#34;en&#34;&gt;
&lt;head&gt;
    &lt;meta charset=&#34;UTF-8&#34;&gt;
    &lt;script type=&#34;application/javascript&#34; src=&#34;https://cdn.jsdelivr.net/npm/vue@3&#34;&gt;&lt;/script&gt;
    &lt;script type=&#34;application/javascript&#34; src=&#34;https://unpkg.com/pytket-circuit-renderer@0.3/dist/pytket-circuit-renderer.umd.js&#34;&gt;&lt;/script&gt;
    &lt;link rel=&#34;stylesheet&#34; href=&#34;https://unpkg.com/pytket-circuit-renderer@0.3/dist/pytket-circuit-renderer.css&#34;&gt;
&lt;/head&gt;
&lt;body&gt;
    &lt;div id=&#34;circuit-display-vue-container-f5fdfb30-c34e-4155-a0fe-98c5820676ff&#34; class=&#34;pytket-circuit-display-container&#34;&gt;
        &lt;div style=&#34;display: none&#34;&gt;
            &lt;div id=&#34;circuit-json-to-display&#34;&gt;{&#34;bits&#34;: [], &#34;commands&#34;: [{&#34;args&#34;: [[&#34;q&#34;, [0]]], &#34;op&#34;: {&#34;type&#34;: &#34;Z&#34;}}, {&#34;args&#34;: [[&#34;q&#34;, [1]]], &#34;op&#34;: {&#34;params&#34;: [&#34;0.25&#34;], &#34;type&#34;: &#34;Rz&#34;}}, {&#34;args&#34;: [[&#34;q&#34;, [1]]], &#34;op&#34;: {&#34;type&#34;: &#34;Z&#34;}}, {&#34;args&#34;: [[&#34;q&#34;, [1]]], &#34;op&#34;: {&#34;type&#34;: &#34;Z&#34;}}], &#34;created_qubits&#34;: [], &#34;discarded_qubits&#34;: [], &#34;implicit_permutation&#34;: [[[&#34;q&#34;, [0]], [&#34;q&#34;, [0]]], [[&#34;q&#34;, [1]], [&#34;q&#34;, [1]]], [[&#34;q&#34;, [2]], [&#34;q&#34;, [2]]]], &#34;phase&#34;: &#34;0.0&#34;, &#34;qubits&#34;: [[&#34;q&#34;, [0]], [&#34;q&#34;, [1]], [&#34;q&#34;, [2]]]}&lt;/div&gt;
        &lt;/div&gt;
        &lt;circuit-display-container :circuit-element-str=&#34;&#39;#circuit-json-to-display&#39;&#34;&gt;&lt;/circuit-display-container&gt;
    &lt;/div&gt;
    &lt;script type=&#34;application/javascript&#34;&gt;
        const { createApp } = Vue;
        const circuitDisplayContainer = window[&#34;pytket-circuit-renderer&#34;].default;
        // Init variables to be shared between circuit display instances
        if (typeof window.pytketCircuitDisplays === &#34;undefined&#34;) {
            window.pytketCircuitDisplays = {};
        }
        const uid = &#34;f5fdfb30-c34e-4155-a0fe-98c5820676ff&#34;;
        // Create the root Vue component
        const app = createApp({
            delimiters: [&#39;[[#&#39;, &#39;#]]&#39;],
            components: { circuitDisplayContainer },
        })
        app.config.unwrapInjectedRef = true;
        app.mount(&#34;#circuit-display-vue-container-&#34;+uid);
        window.pytketCircuitDisplays[uid] = app;
    &lt;/script&gt;
&lt;/body&gt;
&lt;/html&gt;
"
        width="100%" height="200px"
        style="border: none; outline: none; resize: vertical; overflow: auto"></iframe>











<iframe srcdoc="
&lt;!DOCTYPE html&gt;
&lt;html lang=&#34;en&#34;&gt;
&lt;head&gt;
    &lt;meta charset=&#34;UTF-8&#34;&gt;
    &lt;script type=&#34;application/javascript&#34; src=&#34;https://cdn.jsdelivr.net/npm/vue@3&#34;&gt;&lt;/script&gt;
    &lt;script type=&#34;application/javascript&#34; src=&#34;https://unpkg.com/pytket-circuit-renderer@0.3/dist/pytket-circuit-renderer.umd.js&#34;&gt;&lt;/script&gt;
    &lt;link rel=&#34;stylesheet&#34; href=&#34;https://unpkg.com/pytket-circuit-renderer@0.3/dist/pytket-circuit-renderer.css&#34;&gt;
&lt;/head&gt;
&lt;body&gt;
    &lt;div id=&#34;circuit-display-vue-container-c5d01570-6add-4fb9-9a3b-b68dd056c9e9&#34; class=&#34;pytket-circuit-display-container&#34;&gt;
        &lt;div style=&#34;display: none&#34;&gt;
            &lt;div id=&#34;circuit-json-to-display&#34;&gt;{&#34;bits&#34;: [], &#34;commands&#34;: [{&#34;args&#34;: [[&#34;q&#34;, [0]]], &#34;op&#34;: {&#34;params&#34;: [&#34;0&#34;, &#34;0&#34;, &#34;1&#34;], &#34;type&#34;: &#34;TK1&#34;}}, {&#34;args&#34;: [[&#34;q&#34;, [1]]], &#34;op&#34;: {&#34;params&#34;: [&#34;0&#34;, &#34;0&#34;, &#34;0.25&#34;], &#34;type&#34;: &#34;TK1&#34;}}], &#34;created_qubits&#34;: [], &#34;discarded_qubits&#34;: [], &#34;implicit_permutation&#34;: [[[&#34;q&#34;, [0]], [&#34;q&#34;, [0]]], [[&#34;q&#34;, [1]], [&#34;q&#34;, [1]]], [[&#34;q&#34;, [2]], [&#34;q&#34;, [2]]]], &#34;phase&#34;: &#34;0.5&#34;, &#34;qubits&#34;: [[&#34;q&#34;, [0]], [&#34;q&#34;, [1]], [&#34;q&#34;, [2]]]}&lt;/div&gt;
        &lt;/div&gt;
        &lt;circuit-display-container :circuit-element-str=&#34;&#39;#circuit-json-to-display&#39;&#34;&gt;&lt;/circuit-display-container&gt;
    &lt;/div&gt;
    &lt;script type=&#34;application/javascript&#34;&gt;
        const { createApp } = Vue;
        const circuitDisplayContainer = window[&#34;pytket-circuit-renderer&#34;].default;
        // Init variables to be shared between circuit display instances
        if (typeof window.pytketCircuitDisplays === &#34;undefined&#34;) {
            window.pytketCircuitDisplays = {};
        }
        const uid = &#34;c5d01570-6add-4fb9-9a3b-b68dd056c9e9&#34;;
        // Create the root Vue component
        const app = createApp({
            delimiters: [&#39;[[#&#39;, &#39;#]]&#39;],
            components: { circuitDisplayContainer },
        })
        app.config.unwrapInjectedRef = true;
        app.mount(&#34;#circuit-display-vue-container-&#34;+uid);
        window.pytketCircuitDisplays[uid] = app;
    &lt;/script&gt;
&lt;/body&gt;
&lt;/html&gt;
"
        width="100%" height="200px"
        style="border: none; outline: none; resize: vertical; overflow: auto"></iframe>




In this example, we first create a `Circuit` object with three qubits, and apply some Z-phase gates to the first, second, and third qubits using the `Z` and `Rz` methods.

We then create a `CliffordSimp()` pass object and apply it to the circuit using the apply method. The pass will simplify the circuit by applying a set of rules to eliminate redundant gates.

#### 5. Simplify Toffoli gates:

A Toffoli gate is a three-qubit gate that performs a conditional bit flip on the third qubit, based on the states of the first two qubits. It is also sometimes called a Controlled-Controlled-NOT (CCNOT) gate because it is like a Controlled-NOT (CNOT) gate but with an additional control qubit. The Toffoli gate is universal for classical reversible computing, meaning that any classical computation can be expressed as a sequence of Toffoli gates.

In this example, we create a `Circuit` with three qubits, and add a Toffoli gate controlled by qubits 0 and 1, and targeting qubit 2. We then add an `Rz` gate on qubit 1, and another Toffoli gate.


```python
from pytket.circuit import Circuit
from pytket.passes import CliffordSimp

c = Circuit(3)
c.CCX(0, 1, 2)
c.Rz(0.1, 1)
c.CCX(0, 1, 2)

#Render a circuit as jupyter cell output.
render_circuit_jupyter(c)

CliffordSimp().apply(c)

#Render a circuit as jupyter cell output.
render_circuit_jupyter(c)

```








<iframe srcdoc="
&lt;!DOCTYPE html&gt;
&lt;html lang=&#34;en&#34;&gt;
&lt;head&gt;
    &lt;meta charset=&#34;UTF-8&#34;&gt;
    &lt;script type=&#34;application/javascript&#34; src=&#34;https://cdn.jsdelivr.net/npm/vue@3&#34;&gt;&lt;/script&gt;
    &lt;script type=&#34;application/javascript&#34; src=&#34;https://unpkg.com/pytket-circuit-renderer@0.3/dist/pytket-circuit-renderer.umd.js&#34;&gt;&lt;/script&gt;
    &lt;link rel=&#34;stylesheet&#34; href=&#34;https://unpkg.com/pytket-circuit-renderer@0.3/dist/pytket-circuit-renderer.css&#34;&gt;
&lt;/head&gt;
&lt;body&gt;
    &lt;div id=&#34;circuit-display-vue-container-11825a1e-9443-4a89-8381-a6cf2f6fd594&#34; class=&#34;pytket-circuit-display-container&#34;&gt;
        &lt;div style=&#34;display: none&#34;&gt;
            &lt;div id=&#34;circuit-json-to-display&#34;&gt;{&#34;bits&#34;: [], &#34;commands&#34;: [{&#34;args&#34;: [[&#34;q&#34;, [0]], [&#34;q&#34;, [1]], [&#34;q&#34;, [2]]], &#34;op&#34;: {&#34;type&#34;: &#34;CCX&#34;}}, {&#34;args&#34;: [[&#34;q&#34;, [1]]], &#34;op&#34;: {&#34;params&#34;: [&#34;0.1&#34;], &#34;type&#34;: &#34;Rz&#34;}}, {&#34;args&#34;: [[&#34;q&#34;, [0]], [&#34;q&#34;, [1]], [&#34;q&#34;, [2]]], &#34;op&#34;: {&#34;type&#34;: &#34;CCX&#34;}}], &#34;created_qubits&#34;: [], &#34;discarded_qubits&#34;: [], &#34;implicit_permutation&#34;: [[[&#34;q&#34;, [0]], [&#34;q&#34;, [0]]], [[&#34;q&#34;, [1]], [&#34;q&#34;, [1]]], [[&#34;q&#34;, [2]], [&#34;q&#34;, [2]]]], &#34;phase&#34;: &#34;0.0&#34;, &#34;qubits&#34;: [[&#34;q&#34;, [0]], [&#34;q&#34;, [1]], [&#34;q&#34;, [2]]]}&lt;/div&gt;
        &lt;/div&gt;
        &lt;circuit-display-container :circuit-element-str=&#34;&#39;#circuit-json-to-display&#39;&#34;&gt;&lt;/circuit-display-container&gt;
    &lt;/div&gt;
    &lt;script type=&#34;application/javascript&#34;&gt;
        const { createApp } = Vue;
        const circuitDisplayContainer = window[&#34;pytket-circuit-renderer&#34;].default;
        // Init variables to be shared between circuit display instances
        if (typeof window.pytketCircuitDisplays === &#34;undefined&#34;) {
            window.pytketCircuitDisplays = {};
        }
        const uid = &#34;11825a1e-9443-4a89-8381-a6cf2f6fd594&#34;;
        // Create the root Vue component
        const app = createApp({
            delimiters: [&#39;[[#&#39;, &#39;#]]&#39;],
            components: { circuitDisplayContainer },
        })
        app.config.unwrapInjectedRef = true;
        app.mount(&#34;#circuit-display-vue-container-&#34;+uid);
        window.pytketCircuitDisplays[uid] = app;
    &lt;/script&gt;
&lt;/body&gt;
&lt;/html&gt;
"
        width="100%" height="200px"
        style="border: none; outline: none; resize: vertical; overflow: auto"></iframe>











<iframe srcdoc="
&lt;!DOCTYPE html&gt;
&lt;html lang=&#34;en&#34;&gt;
&lt;head&gt;
    &lt;meta charset=&#34;UTF-8&#34;&gt;
    &lt;script type=&#34;application/javascript&#34; src=&#34;https://cdn.jsdelivr.net/npm/vue@3&#34;&gt;&lt;/script&gt;
    &lt;script type=&#34;application/javascript&#34; src=&#34;https://unpkg.com/pytket-circuit-renderer@0.3/dist/pytket-circuit-renderer.umd.js&#34;&gt;&lt;/script&gt;
    &lt;link rel=&#34;stylesheet&#34; href=&#34;https://unpkg.com/pytket-circuit-renderer@0.3/dist/pytket-circuit-renderer.css&#34;&gt;
&lt;/head&gt;
&lt;body&gt;
    &lt;div id=&#34;circuit-display-vue-container-e74c1a0e-222d-4540-a34c-8b295676dae1&#34; class=&#34;pytket-circuit-display-container&#34;&gt;
        &lt;div style=&#34;display: none&#34;&gt;
            &lt;div id=&#34;circuit-json-to-display&#34;&gt;{&#34;bits&#34;: [], &#34;commands&#34;: [{&#34;args&#34;: [[&#34;q&#34;, [2]]], &#34;op&#34;: {&#34;params&#34;: [&#34;1/2&#34;, &#34;1/2&#34;, &#34;1/2&#34;], &#34;type&#34;: &#34;TK1&#34;}}, {&#34;args&#34;: [[&#34;q&#34;, [1]], [&#34;q&#34;, [2]]], &#34;op&#34;: {&#34;type&#34;: &#34;CX&#34;}}, {&#34;args&#34;: [[&#34;q&#34;, [2]]], &#34;op&#34;: {&#34;params&#34;: [&#34;0&#34;, &#34;0&#34;, &#34;-1/4&#34;], &#34;type&#34;: &#34;TK1&#34;}}, {&#34;args&#34;: [[&#34;q&#34;, [0]], [&#34;q&#34;, [2]]], &#34;op&#34;: {&#34;type&#34;: &#34;CX&#34;}}, {&#34;args&#34;: [[&#34;q&#34;, [2]]], &#34;op&#34;: {&#34;params&#34;: [&#34;0&#34;, &#34;0&#34;, &#34;1/4&#34;], &#34;type&#34;: &#34;TK1&#34;}}, {&#34;args&#34;: [[&#34;q&#34;, [1]], [&#34;q&#34;, [2]]], &#34;op&#34;: {&#34;type&#34;: &#34;CX&#34;}}, {&#34;args&#34;: [[&#34;q&#34;, [1]]], &#34;op&#34;: {&#34;params&#34;: [&#34;0&#34;, &#34;0&#34;, &#34;1/4&#34;], &#34;type&#34;: &#34;TK1&#34;}}, {&#34;args&#34;: [[&#34;q&#34;, [2]]], &#34;op&#34;: {&#34;params&#34;: [&#34;0&#34;, &#34;0&#34;, &#34;-1/4&#34;], &#34;type&#34;: &#34;TK1&#34;}}, {&#34;args&#34;: [[&#34;q&#34;, [0]], [&#34;q&#34;, [2]]], &#34;op&#34;: {&#34;type&#34;: &#34;CX&#34;}}, {&#34;args&#34;: [[&#34;q&#34;, [0]], [&#34;q&#34;, [1]]], &#34;op&#34;: {&#34;type&#34;: &#34;CX&#34;}}, {&#34;args&#34;: [[&#34;q&#34;, [2]]], &#34;op&#34;: {&#34;params&#34;: [&#34;0&#34;, &#34;0&#34;, &#34;1/4&#34;], &#34;type&#34;: &#34;TK1&#34;}}, {&#34;args&#34;: [[&#34;q&#34;, [0]]], &#34;op&#34;: {&#34;params&#34;: [&#34;0&#34;, &#34;0&#34;, &#34;1/4&#34;], &#34;type&#34;: &#34;TK1&#34;}}, {&#34;args&#34;: [[&#34;q&#34;, [1]]], &#34;op&#34;: {&#34;params&#34;: [&#34;0&#34;, &#34;0&#34;, &#34;-1/4&#34;], &#34;type&#34;: &#34;TK1&#34;}}, {&#34;args&#34;: [[&#34;q&#34;, [0]], [&#34;q&#34;, [1]]], &#34;op&#34;: {&#34;type&#34;: &#34;CX&#34;}}, {&#34;args&#34;: [[&#34;q&#34;, [1]]], &#34;op&#34;: {&#34;params&#34;: [&#34;0&#34;, &#34;0&#34;, &#34;0.1&#34;], &#34;type&#34;: &#34;TK1&#34;}}, {&#34;args&#34;: [[&#34;q&#34;, [1]], [&#34;q&#34;, [2]]], &#34;op&#34;: {&#34;type&#34;: &#34;CX&#34;}}, {&#34;args&#34;: [[&#34;q&#34;, [2]]], &#34;op&#34;: {&#34;params&#34;: [&#34;0&#34;, &#34;0&#34;, &#34;-1/4&#34;], &#34;type&#34;: &#34;TK1&#34;}}, {&#34;args&#34;: [[&#34;q&#34;, [0]], [&#34;q&#34;, [2]]], &#34;op&#34;: {&#34;type&#34;: &#34;CX&#34;}}, {&#34;args&#34;: [[&#34;q&#34;, [2]]], &#34;op&#34;: {&#34;params&#34;: [&#34;0&#34;, &#34;0&#34;, &#34;1/4&#34;], &#34;type&#34;: &#34;TK1&#34;}}, {&#34;args&#34;: [[&#34;q&#34;, [1]], [&#34;q&#34;, [2]]], &#34;op&#34;: {&#34;type&#34;: &#34;CX&#34;}}, {&#34;args&#34;: [[&#34;q&#34;, [1]]], &#34;op&#34;: {&#34;params&#34;: [&#34;0&#34;, &#34;0&#34;, &#34;1/4&#34;], &#34;type&#34;: &#34;TK1&#34;}}, {&#34;args&#34;: [[&#34;q&#34;, [2]]], &#34;op&#34;: {&#34;params&#34;: [&#34;0&#34;, &#34;0&#34;, &#34;-1/4&#34;], &#34;type&#34;: &#34;TK1&#34;}}, {&#34;args&#34;: [[&#34;q&#34;, [0]], [&#34;q&#34;, [2]]], &#34;op&#34;: {&#34;type&#34;: &#34;CX&#34;}}, {&#34;args&#34;: [[&#34;q&#34;, [0]], [&#34;q&#34;, [1]]], &#34;op&#34;: {&#34;type&#34;: &#34;CX&#34;}}, {&#34;args&#34;: [[&#34;q&#34;, [2]]], &#34;op&#34;: {&#34;params&#34;: [&#34;1/2&#34;, &#34;1/2&#34;, &#34;3/4&#34;], &#34;type&#34;: &#34;TK1&#34;}}, {&#34;args&#34;: [[&#34;q&#34;, [0]]], &#34;op&#34;: {&#34;params&#34;: [&#34;0&#34;, &#34;0&#34;, &#34;1/4&#34;], &#34;type&#34;: &#34;TK1&#34;}}, {&#34;args&#34;: [[&#34;q&#34;, [1]]], &#34;op&#34;: {&#34;params&#34;: [&#34;0&#34;, &#34;0&#34;, &#34;-1/4&#34;], &#34;type&#34;: &#34;TK1&#34;}}, {&#34;args&#34;: [[&#34;q&#34;, [0]], [&#34;q&#34;, [1]]], &#34;op&#34;: {&#34;type&#34;: &#34;CX&#34;}}], &#34;created_qubits&#34;: [], &#34;discarded_qubits&#34;: [], &#34;implicit_permutation&#34;: [[[&#34;q&#34;, [0]], [&#34;q&#34;, [0]]], [[&#34;q&#34;, [1]], [&#34;q&#34;, [1]]], [[&#34;q&#34;, [2]], [&#34;q&#34;, [2]]]], &#34;phase&#34;: &#34;1.25&#34;, &#34;qubits&#34;: [[&#34;q&#34;, [0]], [&#34;q&#34;, [1]], [&#34;q&#34;, [2]]]}&lt;/div&gt;
        &lt;/div&gt;
        &lt;circuit-display-container :circuit-element-str=&#34;&#39;#circuit-json-to-display&#39;&#34;&gt;&lt;/circuit-display-container&gt;
    &lt;/div&gt;
    &lt;script type=&#34;application/javascript&#34;&gt;
        const { createApp } = Vue;
        const circuitDisplayContainer = window[&#34;pytket-circuit-renderer&#34;].default;
        // Init variables to be shared between circuit display instances
        if (typeof window.pytketCircuitDisplays === &#34;undefined&#34;) {
            window.pytketCircuitDisplays = {};
        }
        const uid = &#34;e74c1a0e-222d-4540-a34c-8b295676dae1&#34;;
        // Create the root Vue component
        const app = createApp({
            delimiters: [&#39;[[#&#39;, &#39;#]]&#39;],
            components: { circuitDisplayContainer },
        })
        app.config.unwrapInjectedRef = true;
        app.mount(&#34;#circuit-display-vue-container-&#34;+uid);
        window.pytketCircuitDisplays[uid] = app;
    &lt;/script&gt;
&lt;/body&gt;
&lt;/html&gt;
"
        width="100%" height="200px"
        style="border: none; outline: none; resize: vertical; overflow: auto"></iframe>




As you can see, the two Toffoli gates have been simplified to a `TK1` gate and a controlled-not gate, and the `CliffordSimp()` pass has also commuted the `Rz` gate with the `CCX` gate to move it to an adjacent qubit.

## Conection between the `CliffordSimp()` pass and ZX calculus

The `CliffordSimp()` pass in PyTKET is closely related to the ZX calculus, which is a graphical language for quantum computing that provides a way to reason about quantum circuits containing Clifford gates.

The ZX calculus provides a way to represent quantum circuits graphically using diagrams consisting of nodes and edges, with different types of nodes and edges corresponding to different types of quantum gates. The calculus provides a set of graphical rewriting rules that allow us to simplify and manipulate these diagrams to reason about quantum circuits more efficiently.

The `CliffordSimp()` pass in Pytket applies a similar set of simplification rules to quantum circuits containing Clifford gates. These simplification rules are equivalent to some of the rewriting rules in the ZX calculus, and can be used to simplify a quantum circuit by replacing Clifford gates with equivalent sequences of simpler Clifford gates.

In this way, the `CliffordSimp()` pass can be seen as a tool for applying some of the simplification techniques from the ZX calculus to quantum circuits implemented in software or hardware. By simplifying the circuits in this way, we can make them easier to simulate or implement on quantum hardware.

Overall, the ZX calculus and the `CliffordSimp()` pass are both valuable tools for reasoning about quantum circuits containing Clifford gates, and can be used to simplify and optimize these circuits for more efficient implementation or simulation.
