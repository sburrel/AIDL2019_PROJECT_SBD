---


---

<h1 id="person-verification">Person Verification</h1>
<h2 id="main-goal">Main Goal</h2>
<p>The task that the neural networks designed have to perform is: given two images state if they are from the same identity or not.</p>
<p><img src="https://octodex.github.com/images/yaktocat.png" alt="Image of Yaktocat" width="60" height="50"></p>
<h2 id="dataset">Dataset</h2>
<p>Celebrities in Frontal-Profile dataset has been used. The data set contains 10 frontal and 4 profile images of 500 individuals. Similar to LFW, it has 10 splits defined, each containing 350 same and 350 not-same pairs. This dataset is to be used for face verification.<br>
As Sengupta et al. <sup class="footnote-ref"><a href="#fn1" id="fnref1">1</a></sup> shows that many existing algorithms suffer a decrease over 10% from frontal-frontal to frontal-profile verification, cross-pose face recognition is still an extremely challenging scene.</p>
<h2 id="implemented--arquitecture">Implemented  Arquitecture</h2>
<p>In order to perform the verification task each one of the images passes throught a Siamese Network that uses the same weights while working in tandem on the two different images. For the Siameses Networks we use the convolutional layers of a <a href="https://pytorch.org/docs/stable/torchvision/models.html#classification">network pretrained for image classification</a>. After concatenating the features extracted from the Siameses Networks, we explore two diferent strategies:</p>
<ol>
<li>Use a Decision Netwok to classify the two images as belonging to the same or different identity.</li>
<li>Use a Loss Function that compresses intra-variance (same identity) and enlarges inter-variance (different identities).</li>
</ol>
<div class="mermaid"><svg xmlns="http://www.w3.org/2000/svg" id="mermaid-svg-jUYzzu0CXRswG7UV" width="100%" style="max-width: 906px;" viewBox="0 0 906 546.2999572753906"><g transform="translate(-12, -12)"><g class="output"><g class="clusters"></g><g class="edgePaths"><g class="edgePath" style="opacity: 1;"><path class="path" d="M112.5,66.71665954589844L112.5,91.71665954589844L112.5,116.71665954589844" marker-end="url(#arrowhead355)" style="fill:none"></path><defs><marker id="arrowhead355" viewBox="0 0 10 10" refX="9" refY="5" markerUnits="strokeWidth" markerWidth="8" markerHeight="6" orient="auto"><path d="M 0 0 L 10 5 L 0 10 z" class="arrowheadPath" style="stroke-width: 1px; stroke-dasharray: 1px, 0px;"></path></marker></defs></g><g class="edgePath" style="opacity: 1;"><path class="path" d="M112.5,163.43331909179688L112.5,188.43331909179688L112.5,213.43331909179688" marker-end="url(#arrowhead356)" style="fill:none"></path><defs><marker id="arrowhead356" viewBox="0 0 10 10" refX="9" refY="5" markerUnits="strokeWidth" markerWidth="8" markerHeight="6" orient="auto"><path d="M 0 0 L 10 5 L 0 10 z" class="arrowheadPath" style="stroke-width: 1px; stroke-dasharray: 1px, 0px;"></path></marker></defs></g><g class="edgePath" style="opacity: 1;"><path class="path" d="M347.5,66.71665954589844L347.5,91.71665954589844L347.5,116.71665954589844" marker-end="url(#arrowhead357)" style="fill:none"></path><defs><marker id="arrowhead357" viewBox="0 0 10 10" refX="9" refY="5" markerUnits="strokeWidth" markerWidth="8" markerHeight="6" orient="auto"><path d="M 0 0 L 10 5 L 0 10 z" class="arrowheadPath" style="stroke-width: 1px; stroke-dasharray: 1px, 0px;"></path></marker></defs></g><g class="edgePath" style="opacity: 1;"><path class="path" d="M347.5,163.43331909179688L347.5,188.43331909179688L347.5,213.43331909179688" marker-end="url(#arrowhead358)" style="fill:none"></path><defs><marker id="arrowhead358" viewBox="0 0 10 10" refX="9" refY="5" markerUnits="strokeWidth" markerWidth="8" markerHeight="6" orient="auto"><path d="M 0 0 L 10 5 L 0 10 z" class="arrowheadPath" style="stroke-width: 1px; stroke-dasharray: 1px, 0px;"></path></marker></defs></g><g class="edgePath" style="opacity: 1;"><path class="path" d="M112.5,260.1499786376953L112.5,285.1499786376953L175,310.87249447437046" marker-end="url(#arrowhead359)" style="fill:none"></path><defs><marker id="arrowhead359" viewBox="0 0 10 10" refX="9" refY="5" markerUnits="strokeWidth" markerWidth="8" markerHeight="6" orient="auto"><path d="M 0 0 L 10 5 L 0 10 z" class="arrowheadPath" style="stroke-width: 1px; stroke-dasharray: 1px, 0px;"></path></marker></defs></g><g class="edgePath" style="opacity: 1;"><path class="path" d="M347.5,260.1499786376953L347.5,285.1499786376953L285,310.87249447437046" marker-end="url(#arrowhead360)" style="fill:none"></path><defs><marker id="arrowhead360" viewBox="0 0 10 10" refX="9" refY="5" markerUnits="strokeWidth" markerWidth="8" markerHeight="6" orient="auto"><path d="M 0 0 L 10 5 L 0 10 z" class="arrowheadPath" style="stroke-width: 1px; stroke-dasharray: 1px, 0px;"></path></marker></defs></g><g class="edgePath" style="opacity: 1;"><path class="path" d="M230,356.86663818359375L230,381.86663818359375L230,406.86663818359375" marker-end="url(#arrowhead361)" style="fill:none"></path><defs><marker id="arrowhead361" viewBox="0 0 10 10" refX="9" refY="5" markerUnits="strokeWidth" markerWidth="8" markerHeight="6" orient="auto"><path d="M 0 0 L 10 5 L 0 10 z" class="arrowheadPath" style="stroke-width: 1px; stroke-dasharray: 1px, 0px;"></path></marker></defs></g><g class="edgePath" style="opacity: 1;"><path class="path" d="M230,453.5832977294922L230,478.5832977294922L230,503.5832977294922" marker-end="url(#arrowhead362)" style="fill:none"></path><defs><marker id="arrowhead362" viewBox="0 0 10 10" refX="9" refY="5" markerUnits="strokeWidth" markerWidth="8" markerHeight="6" orient="auto"><path d="M 0 0 L 10 5 L 0 10 z" class="arrowheadPath" style="stroke-width: 1px; stroke-dasharray: 1px, 0px;"></path></marker></defs></g><g class="edgePath" style="opacity: 1;"><path class="path" d="M582.5,66.71665954589844L582.5,91.71665954589844L582.5,116.71665954589844" marker-end="url(#arrowhead363)" style="fill:none"></path><defs><marker id="arrowhead363" viewBox="0 0 10 10" refX="9" refY="5" markerUnits="strokeWidth" markerWidth="8" markerHeight="6" orient="auto"><path d="M 0 0 L 10 5 L 0 10 z" class="arrowheadPath" style="stroke-width: 1px; stroke-dasharray: 1px, 0px;"></path></marker></defs></g><g class="edgePath" style="opacity: 1;"><path class="path" d="M582.5,163.43331909179688L582.5,188.43331909179688L582.5,213.43331909179688" marker-end="url(#arrowhead364)" style="fill:none"></path><defs><marker id="arrowhead364" viewBox="0 0 10 10" refX="9" refY="5" markerUnits="strokeWidth" markerWidth="8" markerHeight="6" orient="auto"><path d="M 0 0 L 10 5 L 0 10 z" class="arrowheadPath" style="stroke-width: 1px; stroke-dasharray: 1px, 0px;"></path></marker></defs></g><g class="edgePath" style="opacity: 1;"><path class="path" d="M582.5,260.1499786376953L582.5,285.1499786376953L582.5,310.1499786376953" marker-end="url(#arrowhead365)" style="fill:none"></path><defs><marker id="arrowhead365" viewBox="0 0 10 10" refX="9" refY="5" markerUnits="strokeWidth" markerWidth="8" markerHeight="6" orient="auto"><path d="M 0 0 L 10 5 L 0 10 z" class="arrowheadPath" style="stroke-width: 1px; stroke-dasharray: 1px, 0px;"></path></marker></defs></g><g class="edgePath" style="opacity: 1;"><path class="path" d="M817.5,66.71665954589844L817.5,91.71665954589844L817.5,116.71665954589844" marker-end="url(#arrowhead366)" style="fill:none"></path><defs><marker id="arrowhead366" viewBox="0 0 10 10" refX="9" refY="5" markerUnits="strokeWidth" markerWidth="8" markerHeight="6" orient="auto"><path d="M 0 0 L 10 5 L 0 10 z" class="arrowheadPath" style="stroke-width: 1px; stroke-dasharray: 1px, 0px;"></path></marker></defs></g><g class="edgePath" style="opacity: 1;"><path class="path" d="M817.5,163.43331909179688L817.5,188.43331909179688L817.5,213.43331909179688" marker-end="url(#arrowhead367)" style="fill:none"></path><defs><marker id="arrowhead367" viewBox="0 0 10 10" refX="9" refY="5" markerUnits="strokeWidth" markerWidth="8" markerHeight="6" orient="auto"><path d="M 0 0 L 10 5 L 0 10 z" class="arrowheadPath" style="stroke-width: 1px; stroke-dasharray: 1px, 0px;"></path></marker></defs></g><g class="edgePath" style="opacity: 1;"><path class="path" d="M817.5,260.1499786376953L817.5,285.1499786376953L817.5,310.1499786376953" marker-end="url(#arrowhead368)" style="fill:none"></path><defs><marker id="arrowhead368" viewBox="0 0 10 10" refX="9" refY="5" markerUnits="strokeWidth" markerWidth="8" markerHeight="6" orient="auto"><path d="M 0 0 L 10 5 L 0 10 z" class="arrowheadPath" style="stroke-width: 1px; stroke-dasharray: 1px, 0px;"></path></marker></defs></g><g class="edgePath" style="opacity: 1;"><path class="path" d="M582.5,356.86663818359375L582.5,381.86663818359375L645,407.5891540202689" marker-end="url(#arrowhead369)" style="fill:none"></path><defs><marker id="arrowhead369" viewBox="0 0 10 10" refX="9" refY="5" markerUnits="strokeWidth" markerWidth="8" markerHeight="6" orient="auto"><path d="M 0 0 L 10 5 L 0 10 z" class="arrowheadPath" style="stroke-width: 1px; stroke-dasharray: 1px, 0px;"></path></marker></defs></g><g class="edgePath" style="opacity: 1;"><path class="path" d="M817.5,356.86663818359375L817.5,381.86663818359375L755,407.5891540202689" marker-end="url(#arrowhead370)" style="fill:none"></path><defs><marker id="arrowhead370" viewBox="0 0 10 10" refX="9" refY="5" markerUnits="strokeWidth" markerWidth="8" markerHeight="6" orient="auto"><path d="M 0 0 L 10 5 L 0 10 z" class="arrowheadPath" style="stroke-width: 1px; stroke-dasharray: 1px, 0px;"></path></marker></defs></g><g class="edgePath" style="opacity: 1;"><path class="path" d="M700,453.5832977294922L700,478.5832977294922L700,503.5832977294922" marker-end="url(#arrowhead371)" style="fill:none"></path><defs><marker id="arrowhead371" viewBox="0 0 10 10" refX="9" refY="5" markerUnits="strokeWidth" markerWidth="8" markerHeight="6" orient="auto"><path d="M 0 0 L 10 5 L 0 10 z" class="arrowheadPath" style="stroke-width: 1px; stroke-dasharray: 1px, 0px;"></path></marker></defs></g></g><g class="edgeLabels"><g class="edgeLabel" style="opacity: 1;" transform=""><g transform="translate(0,0)" class="label"><foreignObject width="0" height="0"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;"><span class="edgeLabel"></span></div></foreignObject></g></g><g class="edgeLabel" style="opacity: 1;" transform=""><g transform="translate(0,0)" class="label"><foreignObject width="0" height="0"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;"><span class="edgeLabel"></span></div></foreignObject></g></g><g class="edgeLabel" style="opacity: 1;" transform=""><g transform="translate(0,0)" class="label"><foreignObject width="0" height="0"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;"><span class="edgeLabel"></span></div></foreignObject></g></g><g class="edgeLabel" style="opacity: 1;" transform=""><g transform="translate(0,0)" class="label"><foreignObject width="0" height="0"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;"><span class="edgeLabel"></span></div></foreignObject></g></g><g class="edgeLabel" style="opacity: 1;" transform=""><g transform="translate(0,0)" class="label"><foreignObject width="0" height="0"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;"><span class="edgeLabel"></span></div></foreignObject></g></g><g class="edgeLabel" style="opacity: 1;" transform=""><g transform="translate(0,0)" class="label"><foreignObject width="0" height="0"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;"><span class="edgeLabel"></span></div></foreignObject></g></g><g class="edgeLabel" style="opacity: 1;" transform=""><g transform="translate(0,0)" class="label"><foreignObject width="0" height="0"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;"><span class="edgeLabel"></span></div></foreignObject></g></g><g class="edgeLabel" style="opacity: 1;" transform=""><g transform="translate(0,0)" class="label"><foreignObject width="0" height="0"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;"><span class="edgeLabel"></span></div></foreignObject></g></g><g class="edgeLabel" style="opacity: 1;" transform=""><g transform="translate(0,0)" class="label"><foreignObject width="0" height="0"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;"><span class="edgeLabel"></span></div></foreignObject></g></g><g class="edgeLabel" style="opacity: 1;" transform=""><g transform="translate(0,0)" class="label"><foreignObject width="0" height="0"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;"><span class="edgeLabel"></span></div></foreignObject></g></g><g class="edgeLabel" style="opacity: 1;" transform=""><g transform="translate(0,0)" class="label"><foreignObject width="0" height="0"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;"><span class="edgeLabel"></span></div></foreignObject></g></g><g class="edgeLabel" style="opacity: 1;" transform=""><g transform="translate(0,0)" class="label"><foreignObject width="0" height="0"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;"><span class="edgeLabel"></span></div></foreignObject></g></g><g class="edgeLabel" style="opacity: 1;" transform=""><g transform="translate(0,0)" class="label"><foreignObject width="0" height="0"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;"><span class="edgeLabel"></span></div></foreignObject></g></g><g class="edgeLabel" style="opacity: 1;" transform=""><g transform="translate(0,0)" class="label"><foreignObject width="0" height="0"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;"><span class="edgeLabel"></span></div></foreignObject></g></g><g class="edgeLabel" style="opacity: 1;" transform=""><g transform="translate(0,0)" class="label"><foreignObject width="0" height="0"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;"><span class="edgeLabel"></span></div></foreignObject></g></g><g class="edgeLabel" style="opacity: 1;" transform=""><g transform="translate(0,0)" class="label"><foreignObject width="0" height="0"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;"><span class="edgeLabel"></span></div></foreignObject></g></g><g class="edgeLabel" style="opacity: 1;" transform=""><g transform="translate(0,0)" class="label"><foreignObject width="0" height="0"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;"><span class="edgeLabel"></span></div></foreignObject></g></g></g><g class="nodes"><g class="node" style="opacity: 1;" id="A" transform="translate(112.5,43.35832977294922)"><rect rx="5" ry="5" x="-37" y="-23.35832977294922" width="74" height="46.71665954589844"></rect><g class="label" transform="translate(0,0)"><g transform="translate(-27,-13.358329772949219)"><foreignObject width="54" height="26.716659545898438"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;">Image 1</div></foreignObject></g></g></g><g class="node" style="opacity: 1;" id="B" transform="translate(112.5,140.07498931884766)"><rect rx="0" ry="0" x="-92.5" y="-23.35832977294922" width="185" height="46.71665954589844"></rect><g class="label" transform="translate(0,0)"><g transform="translate(-82.5,-13.358329772949219)"><foreignObject width="165" height="26.716659545898438"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;">Convolutional Network</div></foreignObject></g></g></g><g class="node" style="opacity: 1;" id="C" transform="translate(112.5,236.7916488647461)"><rect rx="5" ry="5" x="-46.5" y="-23.35832977294922" width="93" height="46.71665954589844"></rect><g class="label" transform="translate(0,0)"><g transform="translate(-36.5,-13.358329772949219)"><foreignObject width="73" height="26.716659545898438"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;">Features 1</div></foreignObject></g></g></g><g class="node" style="opacity: 1;" id="A2" transform="translate(347.5,43.35832977294922)"><rect rx="5" ry="5" x="-37" y="-23.35832977294922" width="74" height="46.71665954589844"></rect><g class="label" transform="translate(0,0)"><g transform="translate(-27,-13.358329772949219)"><foreignObject width="54" height="26.716659545898438"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;">Image 2</div></foreignObject></g></g></g><g class="node" style="opacity: 1;" id="B2" transform="translate(347.5,140.07498931884766)"><rect rx="0" ry="0" x="-92.5" y="-23.35832977294922" width="185" height="46.71665954589844"></rect><g class="label" transform="translate(0,0)"><g transform="translate(-82.5,-13.358329772949219)"><foreignObject width="165" height="26.716659545898438"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;">Convolutional Network</div></foreignObject></g></g></g><g class="node" style="opacity: 1;" id="C2" transform="translate(347.5,236.7916488647461)"><rect rx="5" ry="5" x="-46.5" y="-23.35832977294922" width="93" height="46.71665954589844"></rect><g class="label" transform="translate(0,0)"><g transform="translate(-36.5,-13.358329772949219)"><foreignObject width="73" height="26.716659545898438"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;">Features 2</div></foreignObject></g></g></g><g class="node" style="opacity: 1;" id="D" transform="translate(230,333.50830841064453)"><rect rx="5" ry="5" x="-55" y="-23.35832977294922" width="110" height="46.71665954589844"></rect><g class="label" transform="translate(0,0)"><g transform="translate(-45,-13.358329772949219)"><foreignObject width="90" height="26.716659545898438"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;">Concatenate</div></foreignObject></g></g></g><g class="node" style="opacity: 1;" id="E" transform="translate(230,430.22496795654297)"><rect rx="0" ry="0" x="-73.5" y="-23.35832977294922" width="147" height="46.71665954589844"></rect><g class="label" transform="translate(0,0)"><g transform="translate(-63.5,-13.358329772949219)"><foreignObject width="127" height="26.716659545898438"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;">Decision Network</div></foreignObject></g></g></g><g class="node" style="opacity: 1;" id="F" transform="translate(230,526.9416275024414)"><rect rx="5" ry="5" x="-64.5" y="-23.35832977294922" width="129" height="46.71665954589844"></rect><g class="label" transform="translate(0,0)"><g transform="translate(-54.5,-13.358329772949219)"><foreignObject width="109" height="26.716659545898438"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;">Same/Different</div></foreignObject></g></g></g><g class="node" style="opacity: 1;" id="A3" transform="translate(582.5,43.35832977294922)"><rect rx="5" ry="5" x="-37" y="-23.35832977294922" width="74" height="46.71665954589844"></rect><g class="label" transform="translate(0,0)"><g transform="translate(-27,-13.358329772949219)"><foreignObject width="54" height="26.716659545898438"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;">Image 1</div></foreignObject></g></g></g><g class="node" style="opacity: 1;" id="B3" transform="translate(582.5,140.07498931884766)"><rect rx="0" ry="0" x="-92.5" y="-23.35832977294922" width="185" height="46.71665954589844"></rect><g class="label" transform="translate(0,0)"><g transform="translate(-82.5,-13.358329772949219)"><foreignObject width="165" height="26.716659545898438"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;">Convolutional Network</div></foreignObject></g></g></g><g class="node" style="opacity: 1;" id="C3" transform="translate(582.5,236.7916488647461)"><rect rx="0" ry="0" x="-88" y="-23.35832977294922" width="176" height="46.71665954589844"></rect><g class="label" transform="translate(0,0)"><g transform="translate(-78,-13.358329772949219)"><foreignObject width="156" height="26.716659545898438"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;">Fully Connected Layer</div></foreignObject></g></g></g><g class="node" style="opacity: 1;" id="D3" transform="translate(582.5,333.50830841064453)"><rect rx="5" ry="5" x="-46.5" y="-23.35832977294922" width="93" height="46.71665954589844"></rect><g class="label" transform="translate(0,0)"><g transform="translate(-36.5,-13.358329772949219)"><foreignObject width="73" height="26.716659545898438"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;">Features 1</div></foreignObject></g></g></g><g class="node" style="opacity: 1;" id="A4" transform="translate(817.5,43.35832977294922)"><rect rx="5" ry="5" x="-37" y="-23.35832977294922" width="74" height="46.71665954589844"></rect><g class="label" transform="translate(0,0)"><g transform="translate(-27,-13.358329772949219)"><foreignObject width="54" height="26.716659545898438"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;">Image 2</div></foreignObject></g></g></g><g class="node" style="opacity: 1;" id="B4" transform="translate(817.5,140.07498931884766)"><rect rx="0" ry="0" x="-92.5" y="-23.35832977294922" width="185" height="46.71665954589844"></rect><g class="label" transform="translate(0,0)"><g transform="translate(-82.5,-13.358329772949219)"><foreignObject width="165" height="26.716659545898438"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;">Convolutional Network</div></foreignObject></g></g></g><g class="node" style="opacity: 1;" id="C4" transform="translate(817.5,236.7916488647461)"><rect rx="0" ry="0" x="-88" y="-23.35832977294922" width="176" height="46.71665954589844"></rect><g class="label" transform="translate(0,0)"><g transform="translate(-78,-13.358329772949219)"><foreignObject width="156" height="26.716659545898438"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;">Fully Connected Layer</div></foreignObject></g></g></g><g class="node" style="opacity: 1;" id="D4" transform="translate(817.5,333.50830841064453)"><rect rx="5" ry="5" x="-46.5" y="-23.35832977294922" width="93" height="46.71665954589844"></rect><g class="label" transform="translate(0,0)"><g transform="translate(-36.5,-13.358329772949219)"><foreignObject width="73" height="26.716659545898438"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;">Features 2</div></foreignObject></g></g></g><g class="node" style="opacity: 1;" id="E3" transform="translate(700,430.22496795654297)"><rect rx="5" ry="5" x="-55" y="-23.35832977294922" width="110" height="46.71665954589844"></rect><g class="label" transform="translate(0,0)"><g transform="translate(-45,-13.358329772949219)"><foreignObject width="90" height="26.716659545898438"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;">Concatenate</div></foreignObject></g></g></g><g class="node" style="opacity: 1;" id="F3" transform="translate(700,526.9416275024414)"><rect rx="0" ry="0" x="-92" y="-23.35832977294922" width="184" height="46.71665954589844"></rect><g class="label" transform="translate(0,0)"><g transform="translate(-82,-13.358329772949219)"><foreignObject width="164" height="26.716659545898438"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;">Minimize Loss Function</div></foreignObject></g></g></g></g></g></g></svg></div>
<p>We explore a slight variation of the first architecture, moving one of the fully connected layers to the Siamese’s Networks to extract a more compact representation of the features.</p>
<h3 id="architecture-1">Architecture 1</h3>
<div class="mermaid"><svg xmlns="http://www.w3.org/2000/svg" id="mermaid-svg-nbu83wxLgMxMhLMA" width="100%" style="max-width: 947.8449935913086px;" viewBox="0 0 947.8449935913086 159.43331909179688"><g transform="translate(-12, -12)"><g class="output"><g class="clusters"></g><g class="edgePaths"><g class="edgePath" style="opacity: 1;"><path class="path" d="M94,140.07498931884766L119,140.07498931884766L144,140.07498931884766" marker-end="url(#arrowhead397)" style="fill:none"></path><defs><marker id="arrowhead397" viewBox="0 0 10 10" refX="9" refY="5" markerUnits="strokeWidth" markerWidth="8" markerHeight="6" orient="auto"><path d="M 0 0 L 10 5 L 0 10 z" class="arrowheadPath" style="stroke-width: 1px; stroke-dasharray: 1px, 0px;"></path></marker></defs></g><g class="edgePath" style="opacity: 1;"><path class="path" d="M94,43.35832977294922L119,43.35832977294922L144,43.35832977294922" marker-end="url(#arrowhead398)" style="fill:none"></path><defs><marker id="arrowhead398" viewBox="0 0 10 10" refX="9" refY="5" markerUnits="strokeWidth" markerWidth="8" markerHeight="6" orient="auto"><path d="M 0 0 L 10 5 L 0 10 z" class="arrowheadPath" style="stroke-width: 1px; stroke-dasharray: 1px, 0px;"></path></marker></defs></g><g class="edgePath" style="opacity: 1;"><path class="path" d="M329,43.35832977294922L390.5,43.35832977294922L452,68.88654677755332" marker-end="url(#arrowhead399)" style="fill:none"></path><defs><marker id="arrowhead399" viewBox="0 0 10 10" refX="9" refY="5" markerUnits="strokeWidth" markerWidth="8" markerHeight="6" orient="auto"><path d="M 0 0 L 10 5 L 0 10 z" class="arrowheadPath" style="stroke-width: 1px; stroke-dasharray: 1px, 0px;"></path></marker></defs></g><g class="edgePath" style="opacity: 1;"><path class="path" d="M329,140.07498931884766L390.5,140.07498931884766L452,114.54677231424355" marker-end="url(#arrowhead400)" style="fill:none"></path><defs><marker id="arrowhead400" viewBox="0 0 10 10" refX="9" refY="5" markerUnits="strokeWidth" markerWidth="8" markerHeight="6" orient="auto"><path d="M 0 0 L 10 5 L 0 10 z" class="arrowheadPath" style="stroke-width: 1px; stroke-dasharray: 1px, 0px;"></path></marker></defs></g><g class="edgePath" style="opacity: 1;"><path class="path" d="M562,91.71665954589844L587,91.71665954589844L612,91.71665954589844" marker-end="url(#arrowhead401)" style="fill:none"></path><defs><marker id="arrowhead401" viewBox="0 0 10 10" refX="9" refY="5" markerUnits="strokeWidth" markerWidth="8" markerHeight="6" orient="auto"><path d="M 0 0 L 10 5 L 0 10 z" class="arrowheadPath" style="stroke-width: 1px; stroke-dasharray: 1px, 0px;"></path></marker></defs></g><g class="edgePath" style="opacity: 1;"><path class="path" d="M795,91.71665954589844L820,91.71665954589844L845.4999999999999,92.21665954589844" marker-end="url(#arrowhead402)" style="fill:none"></path><defs><marker id="arrowhead402" viewBox="0 0 10 10" refX="9" refY="5" markerUnits="strokeWidth" markerWidth="8" markerHeight="6" orient="auto"><path d="M 0 0 L 10 5 L 0 10 z" class="arrowheadPath" style="stroke-width: 1px; stroke-dasharray: 1px, 0px;"></path></marker></defs></g></g><g class="edgeLabels"><g class="edgeLabel" style="opacity: 1;" transform=""><g transform="translate(0,0)" class="label"><foreignObject width="0" height="0"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;"><span class="edgeLabel"></span></div></foreignObject></g></g><g class="edgeLabel" style="opacity: 1;" transform=""><g transform="translate(0,0)" class="label"><foreignObject width="0" height="0"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;"><span class="edgeLabel"></span></div></foreignObject></g></g><g class="edgeLabel" style="opacity: 1;" transform="translate(390.5,43.35832977294922)"><g transform="translate(-36.5,-13.358329772949219)" class="label"><foreignObject width="73" height="26.716659545898438"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;"><span class="edgeLabel">Features 1</span></div></foreignObject></g></g><g class="edgeLabel" style="opacity: 1;" transform="translate(390.5,140.07498931884766)"><g transform="translate(-36.5,-13.358329772949219)" class="label"><foreignObject width="73" height="26.716659545898438"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;"><span class="edgeLabel">Features 2</span></div></foreignObject></g></g><g class="edgeLabel" style="opacity: 1;" transform=""><g transform="translate(0,0)" class="label"><foreignObject width="0" height="0"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;"><span class="edgeLabel"></span></div></foreignObject></g></g><g class="edgeLabel" style="opacity: 1;" transform=""><g transform="translate(0,0)" class="label"><foreignObject width="0" height="0"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;"><span class="edgeLabel"></span></div></foreignObject></g></g></g><g class="nodes"><g class="node" style="opacity: 1;" id="A" transform="translate(57,140.07498931884766)"><rect rx="5" ry="5" x="-37" y="-23.35832977294922" width="74" height="46.71665954589844"></rect><g class="label" transform="translate(0,0)"><g transform="translate(-27,-13.358329772949219)"><foreignObject width="54" height="26.716659545898438"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;">Image 2</div></foreignObject></g></g></g><g class="node" style="opacity: 1;" id="B" transform="translate(236.5,140.07498931884766)"><rect rx="0" ry="0" x="-92.5" y="-23.35832977294922" width="185" height="46.71665954589844"></rect><g class="label" transform="translate(0,0)"><g transform="translate(-82.5,-13.358329772949219)"><foreignObject width="165" height="26.716659545898438"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;">Convolutional Network</div></foreignObject></g></g></g><g class="node" style="opacity: 1;" id="A2" transform="translate(57,43.35832977294922)"><rect rx="5" ry="5" x="-37" y="-23.35832977294922" width="74" height="46.71665954589844"></rect><g class="label" transform="translate(0,0)"><g transform="translate(-27,-13.358329772949219)"><foreignObject width="54" height="26.716659545898438"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;">Image 1</div></foreignObject></g></g></g><g class="node" style="opacity: 1;" id="B2" transform="translate(236.5,43.35832977294922)"><rect rx="0" ry="0" x="-92.5" y="-23.35832977294922" width="185" height="46.71665954589844"></rect><g class="label" transform="translate(0,0)"><g transform="translate(-82.5,-13.358329772949219)"><foreignObject width="165" height="26.716659545898438"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;">Convolutional Network</div></foreignObject></g></g></g><g class="node" style="opacity: 1;" id="C" transform="translate(507,91.71665954589844)"><rect rx="0" ry="0" x="-55" y="-23.35832977294922" width="110" height="46.71665954589844"></rect><g class="label" transform="translate(0,0)"><g transform="translate(-45,-13.358329772949219)"><foreignObject width="90" height="26.716659545898438"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;">Concatenate</div></foreignObject></g></g></g><g class="node" style="opacity: 1;" id="D" transform="translate(703.5,91.71665954589844)"><rect rx="0" ry="0" x="-91.5" y="-23.35832977294922" width="183" height="46.71665954589844"></rect><g class="label" transform="translate(0,0)"><g transform="translate(-81.5,-13.358329772949219)"><foreignObject width="163" height="26.716659545898438"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;">Fully Connected Layers</div></foreignObject></g></g></g><g class="node" style="opacity: 1;" id="E" transform="translate(898.4224967956543,91.71665954589844)"><polygon points="53.4224967956543,0 106.8449935913086,-53.4224967956543 53.4224967956543,-106.8449935913086 0,-53.4224967956543" rx="5" ry="5" transform="translate(-53.4224967956543,53.4224967956543)"></polygon><g class="label" transform="translate(0,0)"><g transform="translate(-26,-13.358329772949219)"><foreignObject width="52" height="26.716659545898438"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;">Output</div></foreignObject></g></g></g></g></g></g></svg></div>
<h3 id="architecture-1-variation">Architecture 1, variation</h3>
<div class="mermaid"><svg xmlns="http://www.w3.org/2000/svg" id="mermaid-svg-xsB8Byg87sWH4qqB" width="100%" style="max-width: 1166.8449935913086px;" viewBox="0 0 1166.8449935913086 159.43331909179688"><g transform="translate(-12, -12)"><g class="output"><g class="clusters"></g><g class="edgePaths"><g class="edgePath" style="opacity: 1;"><path class="path" d="M94,43.35832977294922L119,43.35832977294922L144,43.35832977294922" marker-end="url(#arrowhead434)" style="fill:none"></path><defs><marker id="arrowhead434" viewBox="0 0 10 10" refX="9" refY="5" markerUnits="strokeWidth" markerWidth="8" markerHeight="6" orient="auto"><path d="M 0 0 L 10 5 L 0 10 z" class="arrowheadPath" style="stroke-width: 1px; stroke-dasharray: 1px, 0px;"></path></marker></defs></g><g class="edgePath" style="opacity: 1;"><path class="path" d="M329,43.35832977294922L354,43.35832977294922L379,43.35832977294922" marker-end="url(#arrowhead435)" style="fill:none"></path><defs><marker id="arrowhead435" viewBox="0 0 10 10" refX="9" refY="5" markerUnits="strokeWidth" markerWidth="8" markerHeight="6" orient="auto"><path d="M 0 0 L 10 5 L 0 10 z" class="arrowheadPath" style="stroke-width: 1px; stroke-dasharray: 1px, 0px;"></path></marker></defs></g><g class="edgePath" style="opacity: 1;"><path class="path" d="M94,140.07498931884766L119,140.07498931884766L144,140.07498931884766" marker-end="url(#arrowhead436)" style="fill:none"></path><defs><marker id="arrowhead436" viewBox="0 0 10 10" refX="9" refY="5" markerUnits="strokeWidth" markerWidth="8" markerHeight="6" orient="auto"><path d="M 0 0 L 10 5 L 0 10 z" class="arrowheadPath" style="stroke-width: 1px; stroke-dasharray: 1px, 0px;"></path></marker></defs></g><g class="edgePath" style="opacity: 1;"><path class="path" d="M329,140.07498931884766L354,140.07498931884766L379,140.07498931884766" marker-end="url(#arrowhead437)" style="fill:none"></path><defs><marker id="arrowhead437" viewBox="0 0 10 10" refX="9" refY="5" markerUnits="strokeWidth" markerWidth="8" markerHeight="6" orient="auto"><path d="M 0 0 L 10 5 L 0 10 z" class="arrowheadPath" style="stroke-width: 1px; stroke-dasharray: 1px, 0px;"></path></marker></defs></g><g class="edgePath" style="opacity: 1;"><path class="path" d="M548,43.35832977294922L609.5,43.35832977294922L671,68.88654677755332" marker-end="url(#arrowhead438)" style="fill:none"></path><defs><marker id="arrowhead438" viewBox="0 0 10 10" refX="9" refY="5" markerUnits="strokeWidth" markerWidth="8" markerHeight="6" orient="auto"><path d="M 0 0 L 10 5 L 0 10 z" class="arrowheadPath" style="stroke-width: 1px; stroke-dasharray: 1px, 0px;"></path></marker></defs></g><g class="edgePath" style="opacity: 1;"><path class="path" d="M548,140.07498931884766L609.5,140.07498931884766L671,114.54677231424355" marker-end="url(#arrowhead439)" style="fill:none"></path><defs><marker id="arrowhead439" viewBox="0 0 10 10" refX="9" refY="5" markerUnits="strokeWidth" markerWidth="8" markerHeight="6" orient="auto"><path d="M 0 0 L 10 5 L 0 10 z" class="arrowheadPath" style="stroke-width: 1px; stroke-dasharray: 1px, 0px;"></path></marker></defs></g><g class="edgePath" style="opacity: 1;"><path class="path" d="M781,91.71665954589844L806,91.71665954589844L831,91.71665954589844" marker-end="url(#arrowhead440)" style="fill:none"></path><defs><marker id="arrowhead440" viewBox="0 0 10 10" refX="9" refY="5" markerUnits="strokeWidth" markerWidth="8" markerHeight="6" orient="auto"><path d="M 0 0 L 10 5 L 0 10 z" class="arrowheadPath" style="stroke-width: 1px; stroke-dasharray: 1px, 0px;"></path></marker></defs></g><g class="edgePath" style="opacity: 1;"><path class="path" d="M1014,91.71665954589844L1039,91.71665954589844L1064.5,92.21665954589844" marker-end="url(#arrowhead441)" style="fill:none"></path><defs><marker id="arrowhead441" viewBox="0 0 10 10" refX="9" refY="5" markerUnits="strokeWidth" markerWidth="8" markerHeight="6" orient="auto"><path d="M 0 0 L 10 5 L 0 10 z" class="arrowheadPath" style="stroke-width: 1px; stroke-dasharray: 1px, 0px;"></path></marker></defs></g></g><g class="edgeLabels"><g class="edgeLabel" style="opacity: 1;" transform=""><g transform="translate(0,0)" class="label"><foreignObject width="0" height="0"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;"><span class="edgeLabel"></span></div></foreignObject></g></g><g class="edgeLabel" style="opacity: 1;" transform=""><g transform="translate(0,0)" class="label"><foreignObject width="0" height="0"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;"><span class="edgeLabel"></span></div></foreignObject></g></g><g class="edgeLabel" style="opacity: 1;" transform=""><g transform="translate(0,0)" class="label"><foreignObject width="0" height="0"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;"><span class="edgeLabel"></span></div></foreignObject></g></g><g class="edgeLabel" style="opacity: 1;" transform=""><g transform="translate(0,0)" class="label"><foreignObject width="0" height="0"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;"><span class="edgeLabel"></span></div></foreignObject></g></g><g class="edgeLabel" style="opacity: 1;" transform="translate(609.5,43.35832977294922)"><g transform="translate(-36.5,-13.358329772949219)" class="label"><foreignObject width="73" height="26.716659545898438"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;"><span class="edgeLabel">Features 1</span></div></foreignObject></g></g><g class="edgeLabel" style="opacity: 1;" transform="translate(609.5,140.07498931884766)"><g transform="translate(-36.5,-13.358329772949219)" class="label"><foreignObject width="73" height="26.716659545898438"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;"><span class="edgeLabel">Features 2</span></div></foreignObject></g></g><g class="edgeLabel" style="opacity: 1;" transform=""><g transform="translate(0,0)" class="label"><foreignObject width="0" height="0"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;"><span class="edgeLabel"></span></div></foreignObject></g></g><g class="edgeLabel" style="opacity: 1;" transform=""><g transform="translate(0,0)" class="label"><foreignObject width="0" height="0"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;"><span class="edgeLabel"></span></div></foreignObject></g></g></g><g class="nodes"><g class="node" style="opacity: 1;" id="A" transform="translate(57,43.35832977294922)"><rect rx="5" ry="5" x="-37" y="-23.35832977294922" width="74" height="46.71665954589844"></rect><g class="label" transform="translate(0,0)"><g transform="translate(-27,-13.358329772949219)"><foreignObject width="54" height="26.716659545898438"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;">Image 1</div></foreignObject></g></g></g><g class="node" style="opacity: 1;" id="B" transform="translate(236.5,43.35832977294922)"><rect rx="0" ry="0" x="-92.5" y="-23.35832977294922" width="185" height="46.71665954589844"></rect><g class="label" transform="translate(0,0)"><g transform="translate(-82.5,-13.358329772949219)"><foreignObject width="165" height="26.716659545898438"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;">Convolutional Network</div></foreignObject></g></g></g><g class="node" style="opacity: 1;" id="C" transform="translate(463.5,43.35832977294922)"><rect rx="0" ry="0" x="-84.5" y="-23.35832977294922" width="169" height="46.71665954589844"></rect><g class="label" transform="translate(0,0)"><g transform="translate(-74.5,-13.358329772949219)"><foreignObject width="149" height="26.716659545898438"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;">Fully connected layer</div></foreignObject></g></g></g><g class="node" style="opacity: 1;" id="A2" transform="translate(57,140.07498931884766)"><rect rx="5" ry="5" x="-37" y="-23.35832977294922" width="74" height="46.71665954589844"></rect><g class="label" transform="translate(0,0)"><g transform="translate(-27,-13.358329772949219)"><foreignObject width="54" height="26.716659545898438"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;">Image 2</div></foreignObject></g></g></g><g class="node" style="opacity: 1;" id="B2" transform="translate(236.5,140.07498931884766)"><rect rx="0" ry="0" x="-92.5" y="-23.35832977294922" width="185" height="46.71665954589844"></rect><g class="label" transform="translate(0,0)"><g transform="translate(-82.5,-13.358329772949219)"><foreignObject width="165" height="26.716659545898438"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;">Convolutional Network</div></foreignObject></g></g></g><g class="node" style="opacity: 1;" id="C2" transform="translate(463.5,140.07498931884766)"><rect rx="0" ry="0" x="-84.5" y="-23.35832977294922" width="169" height="46.71665954589844"></rect><g class="label" transform="translate(0,0)"><g transform="translate(-74.5,-13.358329772949219)"><foreignObject width="149" height="26.716659545898438"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;">Fully connected layer</div></foreignObject></g></g></g><g class="node" style="opacity: 1;" id="D" transform="translate(726,91.71665954589844)"><rect rx="0" ry="0" x="-55" y="-23.35832977294922" width="110" height="46.71665954589844"></rect><g class="label" transform="translate(0,0)"><g transform="translate(-45,-13.358329772949219)"><foreignObject width="90" height="26.716659545898438"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;">Concatenate</div></foreignObject></g></g></g><g class="node" style="opacity: 1;" id="E" transform="translate(922.5,91.71665954589844)"><rect rx="0" ry="0" x="-91.5" y="-23.35832977294922" width="183" height="46.71665954589844"></rect><g class="label" transform="translate(0,0)"><g transform="translate(-81.5,-13.358329772949219)"><foreignObject width="163" height="26.716659545898438"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;">Fully Connected Layers</div></foreignObject></g></g></g><g class="node" style="opacity: 1;" id="F" transform="translate(1117.4224967956543,91.71665954589844)"><polygon points="53.4224967956543,0 106.8449935913086,-53.4224967956543 53.4224967956543,-106.8449935913086 0,-53.4224967956543" rx="5" ry="5" transform="translate(-53.4224967956543,53.4224967956543)"></polygon><g class="label" transform="translate(0,0)"><g transform="translate(-26,-13.358329772949219)"><foreignObject width="52" height="26.716659545898438"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;">Output</div></foreignObject></g></g></g></g></g></g></svg></div>
<h3 id="architecture-2">Architecture 2</h3>
<div class="mermaid"><svg xmlns="http://www.w3.org/2000/svg" id="mermaid-svg-OqHRDCu9vzHsoFWL" width="100%" style="max-width: 999.8449935913086px;" viewBox="0 0 999.8449935913086 159.43331909179688"><g transform="translate(-12, -12)"><g class="output"><g class="clusters"></g><g class="edgePaths"><g class="edgePath" style="opacity: 1;"><path class="path" d="M94,43.35832977294922L119,43.35832977294922L144,43.35832977294922" marker-end="url(#arrowhead468)" style="fill:none"></path><defs><marker id="arrowhead468" viewBox="0 0 10 10" refX="9" refY="5" markerUnits="strokeWidth" markerWidth="8" markerHeight="6" orient="auto"><path d="M 0 0 L 10 5 L 0 10 z" class="arrowheadPath" style="stroke-width: 1px; stroke-dasharray: 1px, 0px;"></path></marker></defs></g><g class="edgePath" style="opacity: 1;"><path class="path" d="M94,140.07498931884766L119,140.07498931884766L144,140.07498931884766" marker-end="url(#arrowhead469)" style="fill:none"></path><defs><marker id="arrowhead469" viewBox="0 0 10 10" refX="9" refY="5" markerUnits="strokeWidth" markerWidth="8" markerHeight="6" orient="auto"><path d="M 0 0 L 10 5 L 0 10 z" class="arrowheadPath" style="stroke-width: 1px; stroke-dasharray: 1px, 0px;"></path></marker></defs></g><g class="edgePath" style="opacity: 1;"><path class="path" d="M329,43.35832977294922L354,43.35832977294922L379,43.35832977294922" marker-end="url(#arrowhead470)" style="fill:none"></path><defs><marker id="arrowhead470" viewBox="0 0 10 10" refX="9" refY="5" markerUnits="strokeWidth" markerWidth="8" markerHeight="6" orient="auto"><path d="M 0 0 L 10 5 L 0 10 z" class="arrowheadPath" style="stroke-width: 1px; stroke-dasharray: 1px, 0px;"></path></marker></defs></g><g class="edgePath" style="opacity: 1;"><path class="path" d="M329,140.07498931884766L354,140.07498931884766L379,140.07498931884766" marker-end="url(#arrowhead471)" style="fill:none"></path><defs><marker id="arrowhead471" viewBox="0 0 10 10" refX="9" refY="5" markerUnits="strokeWidth" markerWidth="8" markerHeight="6" orient="auto"><path d="M 0 0 L 10 5 L 0 10 z" class="arrowheadPath" style="stroke-width: 1px; stroke-dasharray: 1px, 0px;"></path></marker></defs></g><g class="edgePath" style="opacity: 1;"><path class="path" d="M548,43.35832977294922L609.5,43.35832977294922L686.7876155472742,68.35832977294922" marker-end="url(#arrowhead472)" style="fill:none"></path><defs><marker id="arrowhead472" viewBox="0 0 10 10" refX="9" refY="5" markerUnits="strokeWidth" markerWidth="8" markerHeight="6" orient="auto"><path d="M 0 0 L 10 5 L 0 10 z" class="arrowheadPath" style="stroke-width: 1px; stroke-dasharray: 1px, 0px;"></path></marker></defs></g><g class="edgePath" style="opacity: 1;"><path class="path" d="M548,140.07498931884766L609.5,140.07498931884766L686.7876155472742,115.07498931884766" marker-end="url(#arrowhead473)" style="fill:none"></path><defs><marker id="arrowhead473" viewBox="0 0 10 10" refX="9" refY="5" markerUnits="strokeWidth" markerWidth="8" markerHeight="6" orient="auto"><path d="M 0 0 L 10 5 L 0 10 z" class="arrowheadPath" style="stroke-width: 1px; stroke-dasharray: 1px, 0px;"></path></marker></defs></g><g class="edgePath" style="opacity: 1;"><path class="path" d="M847,91.71665954589844L872,91.71665954589844L897.5,92.21665954589844" marker-end="url(#arrowhead474)" style="fill:none"></path><defs><marker id="arrowhead474" viewBox="0 0 10 10" refX="9" refY="5" markerUnits="strokeWidth" markerWidth="8" markerHeight="6" orient="auto"><path d="M 0 0 L 10 5 L 0 10 z" class="arrowheadPath" style="stroke-width: 1px; stroke-dasharray: 1px, 0px;"></path></marker></defs></g></g><g class="edgeLabels"><g class="edgeLabel" style="opacity: 1;" transform=""><g transform="translate(0,0)" class="label"><foreignObject width="0" height="0"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;"><span class="edgeLabel"></span></div></foreignObject></g></g><g class="edgeLabel" style="opacity: 1;" transform=""><g transform="translate(0,0)" class="label"><foreignObject width="0" height="0"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;"><span class="edgeLabel"></span></div></foreignObject></g></g><g class="edgeLabel" style="opacity: 1;" transform=""><g transform="translate(0,0)" class="label"><foreignObject width="0" height="0"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;"><span class="edgeLabel"></span></div></foreignObject></g></g><g class="edgeLabel" style="opacity: 1;" transform=""><g transform="translate(0,0)" class="label"><foreignObject width="0" height="0"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;"><span class="edgeLabel"></span></div></foreignObject></g></g><g class="edgeLabel" style="opacity: 1;" transform="translate(609.5,43.35832977294922)"><g transform="translate(-36.5,-13.358329772949219)" class="label"><foreignObject width="73" height="26.716659545898438"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;"><span class="edgeLabel">Features 1</span></div></foreignObject></g></g><g class="edgeLabel" style="opacity: 1;" transform="translate(609.5,140.07498931884766)"><g transform="translate(-36.5,-13.358329772949219)" class="label"><foreignObject width="73" height="26.716659545898438"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;"><span class="edgeLabel">Features 2</span></div></foreignObject></g></g><g class="edgeLabel" style="opacity: 1;" transform=""><g transform="translate(0,0)" class="label"><foreignObject width="0" height="0"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;"><span class="edgeLabel"></span></div></foreignObject></g></g></g><g class="nodes"><g class="node" style="opacity: 1;" id="A" transform="translate(57,43.35832977294922)"><rect rx="5" ry="5" x="-37" y="-23.35832977294922" width="74" height="46.71665954589844"></rect><g class="label" transform="translate(0,0)"><g transform="translate(-27,-13.358329772949219)"><foreignObject width="54" height="26.716659545898438"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;">Image 1</div></foreignObject></g></g></g><g class="node" style="opacity: 1;" id="B" transform="translate(236.5,43.35832977294922)"><rect rx="0" ry="0" x="-92.5" y="-23.35832977294922" width="185" height="46.71665954589844"></rect><g class="label" transform="translate(0,0)"><g transform="translate(-82.5,-13.358329772949219)"><foreignObject width="165" height="26.716659545898438"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;">Convolutional Network</div></foreignObject></g></g></g><g class="node" style="opacity: 1;" id="A2" transform="translate(57,140.07498931884766)"><rect rx="5" ry="5" x="-37" y="-23.35832977294922" width="74" height="46.71665954589844"></rect><g class="label" transform="translate(0,0)"><g transform="translate(-27,-13.358329772949219)"><foreignObject width="54" height="26.716659545898438"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;">Image 2</div></foreignObject></g></g></g><g class="node" style="opacity: 1;" id="B2" transform="translate(236.5,140.07498931884766)"><rect rx="0" ry="0" x="-92.5" y="-23.35832977294922" width="185" height="46.71665954589844"></rect><g class="label" transform="translate(0,0)"><g transform="translate(-82.5,-13.358329772949219)"><foreignObject width="165" height="26.716659545898438"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;">Convolutional Network</div></foreignObject></g></g></g><g class="node" style="opacity: 1;" id="C" transform="translate(463.5,43.35832977294922)"><rect rx="0" ry="0" x="-84.5" y="-23.35832977294922" width="169" height="46.71665954589844"></rect><g class="label" transform="translate(0,0)"><g transform="translate(-74.5,-13.358329772949219)"><foreignObject width="149" height="26.716659545898438"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;">Fully connected layer</div></foreignObject></g></g></g><g class="node" style="opacity: 1;" id="C2" transform="translate(463.5,140.07498931884766)"><rect rx="0" ry="0" x="-84.5" y="-23.35832977294922" width="169" height="46.71665954589844"></rect><g class="label" transform="translate(0,0)"><g transform="translate(-74.5,-13.358329772949219)"><foreignObject width="149" height="26.716659545898438"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;">Fully connected layer</div></foreignObject></g></g></g><g class="node" style="opacity: 1;" id="D" transform="translate(759,91.71665954589844)"><rect rx="0" ry="0" x="-88" y="-23.35832977294922" width="176" height="46.71665954589844"></rect><g class="label" transform="translate(0,0)"><g transform="translate(-78,-13.358329772949219)"><foreignObject width="156" height="26.716659545898438"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;">CosineEmbeddingLoss</div></foreignObject></g></g></g><g class="node" style="opacity: 1;" id="E" transform="translate(950.4224967956543,91.71665954589844)"><polygon points="53.4224967956543,0 106.8449935913086,-53.4224967956543 53.4224967956543,-106.8449935913086 0,-53.4224967956543" rx="5" ry="5" transform="translate(-53.4224967956543,53.4224967956543)"></polygon><g class="label" transform="translate(0,0)"><g transform="translate(-26,-13.358329772949219)"><foreignObject width="52" height="26.716659545898438"><div xmlns="http://www.w3.org/1999/xhtml" style="display: inline-block; white-space: nowrap;">Output</div></foreignObject></g></g></g></g></g></g></svg></div>
<h2 id="experiments-performed">Experiments Performed</h2>
<h3 id="studied-parametersstrategies">Studied Parameters/Strategies</h3>
<p>The following parameters/strategies have been studied in order to see its influence in the network and increase the accuracy:</p>
<ol>
<li>Architectures described above</li>
<li>Learning rate</li>
<li>Use of the pre-trained weights of the Convolutional Network</li>
<li>Fine tune some of the weights of the Convolutional Network</li>
<li>Use of different Convolutional Networks</li>
<li>Data Augmentation</li>
<li>Loss Function</li>
</ol>
<p>For the first architecture we have studied the influence of having a single neuron at the end of the network and using <a href="https://pytorch.org/docs/stable/nn.html#torch.nn.BCELoss">Binary Cross Entropy Loss</a> or  having two neurons and optimizing the <a href="https://pytorch.org/docs/stable/nn.html#crossentropyloss">Cross Entropy Loss</a>.</p>
<p>For the second architecture we have used the <a href="https://pytorch.org/docs/stable/nn.html#cosineembeddingloss">Cosine Embedding Loss</a>. It’s a cosine margin based loss that learn to discriminate face features in terms of angular similarity of the feature vectors.</p>
<h3 id="results-obtained">Results Obtained</h3>
<p>We study the influence of data augmentation and the use or not of the pretrained weights at the convolutional network for Architecture 1 and Binary Cross Entropy Loss, see results bellow.</p>

<table>
<thead>
<tr>
<th>Experiment ID</th>
<th align="center">Loss</th>
<th align="center">Features from</th>
<th align="center">Learning Rate</th>
<th align="center">Epochs</th>
<th align="center">Pretrained</th>
<th align="center">Data Augmentation</th>
<th align="center">Validation Accuracy</th>
<th align="center">Test Accuracy</th>
<th align="center">Best Epoch</th>
</tr>
</thead>
<tbody>
<tr>
<td>01</td>
<td align="center">BCELoss</td>
<td align="center">VGG16’s Convolutionals</td>
<td align="center">1e-4</td>
<td align="center">7</td>
<td align="center">Yes</td>
<td align="center">Not</td>
<td align="center">0.711</td>
<td align="center">0.735</td>
<td align="center">4</td>
</tr>
<tr>
<td>02</td>
<td align="center">BCELoss</td>
<td align="center">VGG16’s Convolutionals</td>
<td align="center">1e-4</td>
<td align="center">7</td>
<td align="center">Not</td>
<td align="center">Not</td>
<td align="center">0.506</td>
<td align="center">-</td>
<td align="center">1</td>
</tr>
<tr>
<td>03</td>
<td align="center">BCELoss</td>
<td align="center">VGG16’s Convolutionals</td>
<td align="center">1e-4</td>
<td align="center">7</td>
<td align="center">Yes</td>
<td align="center">Yes</td>
<td align="center">0.709</td>
<td align="center">-</td>
<td align="center">6</td>
</tr>
<tr>
<td>04</td>
<td align="center">BCELoss</td>
<td align="center">VGG16’s Convolutionals</td>
<td align="center">1e-4</td>
<td align="center">7</td>
<td align="center">Not</td>
<td align="center">Yes</td>
<td align="center">0.501</td>
<td align="center">-</td>
<td align="center">7</td>
</tr>
<tr>
<td>05</td>
<td align="center">BCELoss</td>
<td align="center">Linear layer</td>
<td align="center">1e-4</td>
<td align="center">7</td>
<td align="center">Yes</td>
<td align="center">Not</td>
<td align="center">0.714</td>
<td align="center">0.718</td>
<td align="center">3</td>
</tr>
<tr>
<td>06</td>
<td align="center">BCELoss</td>
<td align="center">Linear layer</td>
<td align="center">1e-4</td>
<td align="center">7</td>
<td align="center">Not</td>
<td align="center">Not</td>
<td align="center">0.504</td>
<td align="center">0.505</td>
<td align="center">2</td>
</tr>
<tr>
<td>07</td>
<td align="center">BCELoss</td>
<td align="center">Linear layer</td>
<td align="center">1e-4</td>
<td align="center">7</td>
<td align="center">Yes</td>
<td align="center">Yes</td>
<td align="center">0.713</td>
<td align="center">0.727</td>
<td align="center">4</td>
</tr>
<tr>
<td>08</td>
<td align="center">BCELoss</td>
<td align="center">Linear layer</td>
<td align="center">1e-4</td>
<td align="center">7</td>
<td align="center">Not</td>
<td align="center">Yes</td>
<td align="center">0.501</td>
<td align="center">0.503</td>
<td align="center">1</td>
</tr>
</tbody>
</table><p>Better results are obtained when the training starts with the pretrained weights of the convolutional network.</p>
<p>We continue exploring the influence of data augmentation for Architecture 1 and Cross Entropy Loss, see results bellow.</p>

<table>
<thead>
<tr>
<th>Experiment ID</th>
<th align="center">Loss</th>
<th align="center">Features from</th>
<th align="center">Learning Rate</th>
<th align="center">Epochs</th>
<th align="center">Pretrained</th>
<th align="center">Data Augmentation</th>
<th align="center">Validation Accuracy</th>
<th align="center">Test Accuracy</th>
<th align="center">Best Epoch</th>
</tr>
</thead>
<tbody>
<tr>
<td>11</td>
<td align="center">CELoss</td>
<td align="center">VGG16’s Convolutionals</td>
<td align="center">1e-4</td>
<td align="center">20</td>
<td align="center">Yes</td>
<td align="center">Not</td>
<td align="center">0.793</td>
<td align="center">0.782</td>
<td align="center">11</td>
</tr>
<tr>
<td>13</td>
<td align="center">CELoss</td>
<td align="center">VGG16’s Convolutionals</td>
<td align="center">1e-4</td>
<td align="center">14</td>
<td align="center">Yes</td>
<td align="center">Yes</td>
<td align="center">0.803</td>
<td align="center">0.819</td>
<td align="center">11</td>
</tr>
<tr>
<td>15</td>
<td align="center">CELoss</td>
<td align="center">Linear layer</td>
<td align="center">1e-4</td>
<td align="center">20</td>
<td align="center">Yes</td>
<td align="center">Not</td>
<td align="center">0.772</td>
<td align="center">0.765</td>
<td align="center">14</td>
</tr>
<tr>
<td>17</td>
<td align="center">CELoss</td>
<td align="center">Linear layer</td>
<td align="center">1e-4</td>
<td align="center">14</td>
<td align="center">Yes</td>
<td align="center">Yes</td>
<td align="center">0.710</td>
<td align="center">0.727</td>
<td align="center">13</td>
</tr>
</tbody>
</table><p>Data augmentation increases accuracy around 1-4% when the input features of the decision network are extracted from the convolutional layers of the VGG16 and the Cross Entropy Loss is used.</p>
<p>We study the influence of the learning rate for Architecture 1 and Cross Entropy Loss, see results bellow.</p>

<table>
<thead>
<tr>
<th>Experiment ID</th>
<th align="center">Loss</th>
<th align="center">Features from</th>
<th align="center">Learning Rate</th>
<th align="center">Epochs</th>
<th align="center">Pretrained</th>
<th align="center">Data Augmentation</th>
<th align="center">Validation Accuracy</th>
<th align="center">Test Accuracy</th>
<th align="center">Best Epoch</th>
</tr>
</thead>
<tbody>
<tr>
<td>13</td>
<td align="center">CELoss</td>
<td align="center">VGG16’s Convolutionals</td>
<td align="center">1e-4</td>
<td align="center">20</td>
<td align="center">Yes</td>
<td align="center">Yes</td>
<td align="center">0.718</td>
<td align="center">0.730</td>
<td align="center">20</td>
</tr>
<tr>
<td>13</td>
<td align="center">CELoss</td>
<td align="center">VGG16’s Convolutionals</td>
<td align="center">5e-4</td>
<td align="center">20</td>
<td align="center">Yes</td>
<td align="center">Yes</td>
<td align="center">0.825</td>
<td align="center">0.824</td>
<td align="center">16</td>
</tr>
<tr>
<td>13</td>
<td align="center">CELoss</td>
<td align="center">VGG16’s Convolutionals</td>
<td align="center">1e-3</td>
<td align="center">20</td>
<td align="center">Yes</td>
<td align="center">Yes</td>
<td align="center">0.826</td>
<td align="center">0.834</td>
<td align="center">20</td>
</tr>
</tbody>
</table><p>When training the network with a learning rate of 5e-4 or 1e-3 we face some problems for convergence with the Adam Optimizer. The results reflected at the table above were obtained with the SGD Optimizer.</p>
<p>We train the Architecture 1 with Cross Entropy Loss during more epochs, see results bellow.</p>

<table>
<thead>
<tr>
<th>Experiment ID</th>
<th align="center">Loss</th>
<th align="center">Features from</th>
<th align="center">Learning Rate</th>
<th align="center">Epochs</th>
<th align="center">Pretrained</th>
<th align="center">Data Augmentation</th>
<th align="center">Validation Accuracy</th>
<th align="center">Test Accuracy</th>
<th align="center">Best Epoch</th>
</tr>
</thead>
<tbody>
<tr>
<td>13</td>
<td align="center">CELoss</td>
<td align="center">VGG16’s Convolutionals</td>
<td align="center">1e-4</td>
<td align="center">40</td>
<td align="center">Yes</td>
<td align="center">Yes</td>
<td align="center">0.827</td>
<td align="center">0.832</td>
<td align="center">39</td>
</tr>
</tbody>
</table><p>When training for a greater number of epochs we obtain a greater accuracy.</p>
<p>The following experiments have been performed with the Architecture 2:</p>

<table>
<thead>
<tr>
<th>Experiment ID</th>
<th align="center">Loss</th>
<th align="center">Features from</th>
<th align="center">Learning Rate</th>
<th align="center">Epochs</th>
<th align="center">Pretrained</th>
<th align="center">Data Augmentation</th>
<th align="center">Validation Accuracy</th>
<th align="center">Test Accuracy</th>
<th align="center">Best Epoch</th>
</tr>
</thead>
<tbody>
<tr>
<td>21</td>
<td align="center">CosineEmbeddingLoss</td>
<td align="center">VGG16’s Convolutionals</td>
<td align="center">1e-4</td>
<td align="center">14</td>
<td align="center">Yes</td>
<td align="center">Not</td>
<td align="center">0.807</td>
<td align="center">0.826</td>
<td align="center">9</td>
</tr>
<tr>
<td>21</td>
<td align="center">CosineEmbeddingLoss</td>
<td align="center">VGG16’s Convolutionals</td>
<td align="center">5e-4</td>
<td align="center">14</td>
<td align="center">Yes</td>
<td align="center">Not</td>
<td align="center">0.708</td>
<td align="center">0.714</td>
<td align="center">11</td>
</tr>
<tr>
<td>23</td>
<td align="center">CosineEmbeddingLoss</td>
<td align="center">VGG16’s Convolutionals</td>
<td align="center">1e-4</td>
<td align="center">14</td>
<td align="center">Yes</td>
<td align="center">Yes</td>
<td align="center">0.821</td>
<td align="center">0.818</td>
<td align="center">12</td>
</tr>
<tr>
<td>23</td>
<td align="center">CosineEmbeddingLoss</td>
<td align="center">VGG16’s Convolutionals</td>
<td align="center">5e-4</td>
<td align="center">14</td>
<td align="center">Yes</td>
<td align="center">Yes</td>
<td align="center">0.709</td>
<td align="center">0.720</td>
<td align="center">14</td>
</tr>
</tbody>
</table><p>We train the Architecture 2 during more epochs, see results bellow.</p>

<table>
<thead>
<tr>
<th>Experiment ID</th>
<th align="center">Loss</th>
<th align="center">Features from</th>
<th align="center">Learning Rate</th>
<th align="center">Epochs</th>
<th align="center">Pretrained</th>
<th align="center">Data Augmentation</th>
<th align="center">Validation Accuracy</th>
<th align="center">Test Accuracy</th>
<th align="center">Best Epoch</th>
</tr>
</thead>
<tbody>
<tr>
<td>23 executant a S_E</td>
<td align="center">CosineEmbeddingLoss</td>
<td align="center">VGG16’s Convolutionals</td>
<td align="center">1e-4</td>
<td align="center">40</td>
<td align="center">Yes</td>
<td align="center">Yes</td>
<td align="center"></td>
<td align="center"></td>
<td align="center"></td>
</tr>
</tbody>
</table><p>We try to fine tune only half of the layers of the VGG16 instead of training the whole network for Architecture 2.</p>

<table>
<thead>
<tr>
<th>Experiment ID</th>
<th align="center">Loss</th>
<th align="center">Features from</th>
<th align="center">Learning Rate</th>
<th align="center">Epochs</th>
<th align="center">Pretrained</th>
<th align="center">Data Augmentation</th>
<th align="center">Validation Accuracy</th>
<th align="center">Test Accuracy</th>
<th align="center">Best Epoch</th>
</tr>
</thead>
<tbody>
<tr>
<td>33</td>
<td align="center">CosineEmbeddingLoss</td>
<td align="center">VGG16’s Convolutionals Fine Tunning</td>
<td align="center">1e-4</td>
<td align="center">14</td>
<td align="center">Yes</td>
<td align="center">Yes</td>
<td align="center">0.746</td>
<td align="center">0.749</td>
<td align="center">13</td>
</tr>
</tbody>
</table><p>The fine tuning strategy doesn’t get better results for the studied case.</p>
<p>We try different Convolutional Networks for Architecture 2.</p>

<table>
<thead>
<tr>
<th>Experiment ID</th>
<th align="center">Loss</th>
<th align="center">Features from</th>
<th align="center">Learning Rate</th>
<th align="center">Epochs</th>
<th align="center">Pretrained</th>
<th align="center">Data Augmentation</th>
<th align="center">Validation Accuracy</th>
<th align="center">Test Accuracy</th>
<th align="center">Best Epoch</th>
</tr>
</thead>
<tbody>
<tr>
<td>43</td>
<td align="center">CosineEmbeddingLoss</td>
<td align="center">ResNet50</td>
<td align="center">1e-4</td>
<td align="center">14</td>
<td align="center">Yes</td>
<td align="center">Yes</td>
<td align="center">0.822</td>
<td align="center">0.825</td>
<td align="center">7</td>
</tr>
<tr>
<td>53</td>
<td align="center">CosineEmbeddingLoss</td>
<td align="center">ResNet101</td>
<td align="center">1e-4</td>
<td align="center">14</td>
<td align="center">Yes</td>
<td align="center">Yes</td>
<td align="center">0.823</td>
<td align="center">0.831</td>
<td align="center">14</td>
</tr>
<tr>
<td>63</td>
<td align="center">CosineEmbeddingLoss</td>
<td align="center">ResNext50</td>
<td align="center">1e-4</td>
<td align="center">14</td>
<td align="center">Yes</td>
<td align="center">Yes</td>
<td align="center">0.826</td>
<td align="center">0.834</td>
<td align="center">12</td>
</tr>
<tr>
<td>73</td>
<td align="center">CosineEmbeddingLoss</td>
<td align="center">ResNext101</td>
<td align="center">1e-4</td>
<td align="center">14</td>
<td align="center">Yes</td>
<td align="center">Yes</td>
<td align="center"></td>
<td align="center"></td>
<td align="center"></td>
</tr>
</tbody>
</table><h3 id="observed-behavior">Observed Behavior</h3>
<h3 id="how-to-reproduce-the-experiments">How to reproduce the experiments</h3>
<h2 id="conclusions">Conclusions</h2>
<h2 id="to-do">TO DO</h2>
<h2 id="inspiration">Inspiration</h2>
<p>Inheriting from the object classification network such as AlexNet, the initial Deepface and DeepID adopted cross-entropy based softmax loss for feature learning. After that, people realizad that the softmax loss is not sufficient by itself to learn feature with large margin, and more researchers began to explore discriminative loss functions for enhanced generalization ability. Before 2017, Euclidean-distance-based loss played an important role; in 2017, angular/cosine-margin-based loss as well as feature and weight normalization became popular.<br>
DeepFace<br>
FaceNet</p>
<blockquote>
<p>Written with <a href="https://stackedit.io/">StackEdit</a>.</p>
</blockquote>
<hr class="footnotes-sep">
<section class="footnotes">
<ol class="footnotes-list">
<li id="fn1" class="footnote-item"><p>S. Sengupta, J.C. Cheng, C.D. Castillo, V.M. Patel, R. Chellappa and  D.W. Jacobs.  <strong><a href="http://www.cfpw.io/paper.pdf">Frontal to Profile Face Verification in the Wild</a></strong>, IEEE Conference on Applications of Computer Vision, 2016. <a href="#fnref1" class="footnote-backref">↩︎</a></p>
</li>
</ol>
</section>

