# VC-Project

{::nomarkdown}

<!DOCTYPE html>
<html>
<head><meta charset="utf-8" />
<title>Proyecto_VC</title><script src="https://cdnjs.cloudflare.com/ajax/libs/require.js/2.1.10/require.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/2.0.3/jquery.min.js"></script>

<body>
  <div tabindex="-1" id="notebook" class="border-box-sizing">
    <div class="container" id="notebook-container">

<div class="cell border-box-sizing text_cell rendered">
<div class="prompt input_prompt">
</div>
<div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<h1 id="Libraries-used-and-auxiliary-functions">Libraries used and auxiliary functions<a class="anchor-link" href="#Libraries-used-and-auxiliary-functions">&#182;</a></h1>
</div>
</div>
</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[1]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="kn">import</span> <span class="nn">numpy</span> <span class="k">as</span> <span class="nn">np</span>
<span class="kn">import</span> <span class="nn">cv2</span>
<span class="kn">import</span> <span class="nn">pandas</span> <span class="k">as</span> <span class="nn">pd</span>

<span class="kn">import</span> <span class="nn">joblib</span>
<span class="kn">import</span> <span class="nn">threading</span>
<span class="kn">import</span> <span class="nn">datetime</span>
<span class="kn">from</span> <span class="nn">matplotlib</span> <span class="k">import</span> <span class="n">pyplot</span> <span class="k">as</span> <span class="n">plt</span>
</pre></div>

</div>
</div>
</div>

</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[2]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="n">PYPLOT</span> <span class="o">=</span> <span class="mi">0</span>
<span class="n">IMSHOW</span> <span class="o">=</span> <span class="mi">1</span>
<span class="c1"># Utility function to show an image, closes when ESC is pressed</span>
<span class="k">def</span> <span class="nf">show_image</span><span class="p">(</span><span class="n">img_input</span><span class="p">,</span> <span class="n">label</span><span class="o">=</span><span class="s1">&#39;Image&#39;</span><span class="p">,</span> <span class="n">method</span><span class="o">=</span><span class="mi">0</span><span class="p">):</span>
    <span class="k">if</span> <span class="n">method</span><span class="o">==</span><span class="n">PYPLOT</span><span class="p">:</span>
        <span class="n">plt</span><span class="o">.</span><span class="n">axis</span><span class="p">(</span><span class="s2">&quot;off&quot;</span><span class="p">)</span>
        <span class="k">if</span> <span class="nb">len</span><span class="p">(</span><span class="n">img_input</span><span class="o">.</span><span class="n">shape</span><span class="p">)</span> <span class="o">==</span> <span class="mi">3</span><span class="p">:</span>
            <span class="n">plt</span><span class="o">.</span><span class="n">imshow</span><span class="p">(</span><span class="n">cv2</span><span class="o">.</span><span class="n">cvtColor</span><span class="p">(</span><span class="n">img_input</span><span class="p">,</span> <span class="n">cv2</span><span class="o">.</span><span class="n">COLOR_BGR2RGB</span><span class="p">))</span>
        <span class="k">else</span><span class="p">:</span>
            <span class="n">plt</span><span class="o">.</span><span class="n">imshow</span><span class="p">(</span><span class="n">img_input</span><span class="p">,</span> <span class="n">cmap</span><span class="o">=</span><span class="s1">&#39;Greys_r&#39;</span><span class="p">)</span>

        <span class="n">plt</span><span class="o">.</span><span class="n">show</span><span class="p">()</span>
    <span class="k">elif</span> <span class="n">method</span><span class="o">==</span><span class="n">IMSHOW</span><span class="p">:</span>
        <span class="n">cv2</span><span class="o">.</span><span class="n">destroyAllWindows</span><span class="p">()</span>
        <span class="n">cv2</span><span class="o">.</span><span class="n">namedWindow</span><span class="p">(</span><span class="n">label</span><span class="p">,</span><span class="n">cv2</span><span class="o">.</span><span class="n">WINDOW_NORMAL</span><span class="p">)</span>
        <span class="n">cv2</span><span class="o">.</span><span class="n">resizeWindow</span><span class="p">(</span><span class="n">label</span><span class="p">,</span><span class="mi">800</span><span class="p">,</span><span class="mi">600</span><span class="p">)</span>
        <span class="k">if</span> <span class="n">img_input</span><span class="o">.</span><span class="n">nonzero</span><span class="p">():</span>
            <span class="n">cv2</span><span class="o">.</span><span class="n">imshow</span><span class="p">(</span><span class="n">label</span><span class="p">,</span> <span class="n">img_input</span><span class="p">)</span>
        <span class="k">else</span><span class="p">:</span>
            <span class="n">cv2</span><span class="o">.</span><span class="n">destroyAllWindows</span><span class="p">()</span>
            <span class="k">return</span>

        <span class="n">k</span> <span class="o">=</span> <span class="n">cv2</span><span class="o">.</span><span class="n">waitKey</span><span class="p">(</span><span class="mi">0</span><span class="p">)</span>

        <span class="n">cv2</span><span class="o">.</span><span class="n">destroyAllWindows</span><span class="p">()</span>
    <span class="k">return</span>
</pre></div>

</div>
</div>
</div>

</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[3]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="c1"># Load a rng seed for reproducibility</span>
<span class="n">cv2</span><span class="o">.</span><span class="n">setNumThreads</span><span class="p">(</span><span class="n">joblib</span><span class="o">.</span><span class="n">cpu_count</span><span class="p">())</span>
<span class="n">np</span><span class="o">.</span><span class="n">random</span><span class="o">.</span><span class="n">seed</span><span class="p">(</span><span class="mi">0</span><span class="p">)</span>

<span class="c1"># Set the detector of choice; 0 for KAZE, 1 for SURF</span>
<span class="n">detector_choice</span> <span class="o">=</span> <span class="mi">1</span>
</pre></div>

</div>
</div>
</div>

</div>
<div class="cell border-box-sizing text_cell rendered">
<div class="prompt input_prompt">
</div>
<div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<h1 id="Data-loading-and-metrics">Data loading and metrics<a class="anchor-link" href="#Data-loading-and-metrics">&#182;</a></h1>
</div>
</div>
</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[4]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="c1"># Load the data into a dataframe</span>
<span class="n">path_root_db</span> <span class="o">=</span> <span class="s2">&quot;./signDatabasePublicFramesOnly/&quot;</span>
<span class="n">path_file</span> <span class="o">=</span> <span class="s2">&quot;allAnnotations.csv&quot;</span>
<span class="n">data_frame</span> <span class="o">=</span> <span class="n">pd</span><span class="o">.</span><span class="n">read_csv</span><span class="p">(</span><span class="n">path_root_db</span> <span class="o">+</span> <span class="n">path_file</span><span class="p">,</span> <span class="n">sep</span><span class="o">=</span><span class="s1">&#39;;|,&#39;</span><span class="p">,</span> <span class="n">engine</span><span class="o">=</span><span class="s1">&#39;python&#39;</span><span class="p">)</span>

<span class="c1"># Add the full path to the file name</span>
<span class="n">data_frame</span><span class="p">[</span><span class="s2">&quot;Filename&quot;</span><span class="p">]</span> <span class="o">=</span> <span class="n">path_root_db</span><span class="o">+</span><span class="n">data_frame</span><span class="p">[</span><span class="s2">&quot;Filename&quot;</span><span class="p">]</span>

<span class="c1"># Add a label column</span>
<span class="n">sign_categories</span> <span class="o">=</span> <span class="n">data_frame</span><span class="p">[</span><span class="s2">&quot;Annotation tag&quot;</span><span class="p">]</span><span class="o">.</span><span class="n">astype</span><span class="p">(</span><span class="s2">&quot;category&quot;</span><span class="p">,</span> <span class="n">ordered</span> <span class="o">=</span> <span class="kc">False</span><span class="p">)</span><span class="o">.</span><span class="n">cat</span>
<span class="n">data_frame</span><span class="p">[</span><span class="s2">&quot;label&quot;</span><span class="p">]</span> <span class="o">=</span> <span class="n">sign_categories</span><span class="o">.</span><span class="n">codes</span>

<span class="n">df</span> <span class="o">=</span> <span class="n">data_frame</span><span class="o">.</span><span class="n">copy</span><span class="p">()</span>
</pre></div>

</div>
</div>
</div>

</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[5]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="n">data_frame</span>
</pre></div>

</div>
</div>
</div>

<div class="output_wrapper">
<div class="output">


<div class="output_area">
<div class="prompt output_prompt">Out[5]:</div>


<div class="output_html rendered_html output_subarea output_execute_result">
<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Filename</th>
      <th>Annotation tag</th>
      <th>Upper left corner X</th>
      <th>Upper left corner Y</th>
      <th>Lower right corner X</th>
      <th>Lower right corner Y</th>
      <th>Occluded</th>
      <th>On another road</th>
      <th>Origin file</th>
      <th>Origin frame number</th>
      <th>Origin track</th>
      <th>Origin track frame number</th>
      <th>label</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>./signDatabasePublicFramesOnly/aiua120214-0/fr...</td>
      <td>stop</td>
      <td>862</td>
      <td>104</td>
      <td>916</td>
      <td>158</td>
      <td>0</td>
      <td>0</td>
      <td>aiua120214-0/DataLog02142012_external_camera.avi</td>
      <td>2667</td>
      <td>stop_1330545910.avi</td>
      <td>2</td>
      <td>35</td>
    </tr>
    <tr>
      <th>1</th>
      <td>./signDatabasePublicFramesOnly/aiua120214-0/fr...</td>
      <td>speedLimitUrdbl</td>
      <td>425</td>
      <td>197</td>
      <td>438</td>
      <td>213</td>
      <td>0</td>
      <td>0</td>
      <td>aiua120214-0/DataLog02142012_external_camera.avi</td>
      <td>2667</td>
      <td>stop_1330545910.avi</td>
      <td>2</td>
      <td>34</td>
    </tr>
    <tr>
      <th>2</th>
      <td>./signDatabasePublicFramesOnly/aiua120214-0/fr...</td>
      <td>stop</td>
      <td>922</td>
      <td>88</td>
      <td>982</td>
      <td>148</td>
      <td>1</td>
      <td>0</td>
      <td>aiua120214-0/DataLog02142012_external_camera.avi</td>
      <td>2672</td>
      <td>stop_1330545910.avi</td>
      <td>7</td>
      <td>35</td>
    </tr>
    <tr>
      <th>3</th>
      <td>./signDatabasePublicFramesOnly/aiua120214-0/fr...</td>
      <td>speedLimit25</td>
      <td>447</td>
      <td>193</td>
      <td>461</td>
      <td>210</td>
      <td>0</td>
      <td>0</td>
      <td>aiua120214-0/DataLog02142012_external_camera.avi</td>
      <td>2672</td>
      <td>stop_1330545910.avi</td>
      <td>7</td>
      <td>26</td>
    </tr>
    <tr>
      <th>4</th>
      <td>./signDatabasePublicFramesOnly/aiua120214-0/fr...</td>
      <td>speedLimit25</td>
      <td>469</td>
      <td>189</td>
      <td>483</td>
      <td>207</td>
      <td>0</td>
      <td>0</td>
      <td>aiua120214-0/DataLog02142012_external_camera.avi</td>
      <td>2677</td>
      <td>stop_1330545910.avi</td>
      <td>12</td>
      <td>26</td>
    </tr>
    <tr>
      <th>5</th>
      <td>./signDatabasePublicFramesOnly/aiua120214-0/fr...</td>
      <td>speedLimit25</td>
      <td>529</td>
      <td>183</td>
      <td>546</td>
      <td>203</td>
      <td>0</td>
      <td>0</td>
      <td>aiua120214-0/DataLog02142012_external_camera.avi</td>
      <td>2717</td>
      <td>speedLimit_1330545914.avi</td>
      <td>2</td>
      <td>26</td>
    </tr>
    <tr>
      <th>6</th>
      <td>./signDatabasePublicFramesOnly/aiua120214-0/fr...</td>
      <td>speedLimit25</td>
      <td>526</td>
      <td>191</td>
      <td>543</td>
      <td>211</td>
      <td>1</td>
      <td>0</td>
      <td>aiua120214-0/DataLog02142012_external_camera.avi</td>
      <td>2724</td>
      <td>speedLimit_1330545914.avi</td>
      <td>9</td>
      <td>26</td>
    </tr>
    <tr>
      <th>7</th>
      <td>./signDatabasePublicFramesOnly/aiua120214-0/fr...</td>
      <td>speedLimit25</td>
      <td>522</td>
      <td>189</td>
      <td>540</td>
      <td>210</td>
      <td>1</td>
      <td>0</td>
      <td>aiua120214-0/DataLog02142012_external_camera.avi</td>
      <td>2731</td>
      <td>speedLimit_1330545914.avi</td>
      <td>16</td>
      <td>26</td>
    </tr>
    <tr>
      <th>8</th>
      <td>./signDatabasePublicFramesOnly/aiua120214-0/fr...</td>
      <td>speedLimit25</td>
      <td>518</td>
      <td>191</td>
      <td>536</td>
      <td>213</td>
      <td>0</td>
      <td>0</td>
      <td>aiua120214-0/DataLog02142012_external_camera.avi</td>
      <td>2738</td>
      <td>speedLimit_1330545914.avi</td>
      <td>23</td>
      <td>26</td>
    </tr>
    <tr>
      <th>9</th>
      <td>./signDatabasePublicFramesOnly/aiua120214-0/fr...</td>
      <td>speedLimit25</td>
      <td>515</td>
      <td>191</td>
      <td>533</td>
      <td>213</td>
      <td>0</td>
      <td>0</td>
      <td>aiua120214-0/DataLog02142012_external_camera.avi</td>
      <td>2745</td>
      <td>speedLimit_1330545914.avi</td>
      <td>30</td>
      <td>26</td>
    </tr>
    <tr>
      <th>10</th>
      <td>./signDatabasePublicFramesOnly/aiua120214-0/fr...</td>
      <td>speedLimit25</td>
      <td>512</td>
      <td>191</td>
      <td>531</td>
      <td>213</td>
      <td>0</td>
      <td>0</td>
      <td>aiua120214-0/DataLog02142012_external_camera.avi</td>
      <td>2752</td>
      <td>speedLimit_1330545914.avi</td>
      <td>37</td>
      <td>26</td>
    </tr>
    <tr>
      <th>11</th>
      <td>./signDatabasePublicFramesOnly/aiua120214-0/fr...</td>
      <td>speedLimit25</td>
      <td>511</td>
      <td>191</td>
      <td>530</td>
      <td>213</td>
      <td>0</td>
      <td>0</td>
      <td>aiua120214-0/DataLog02142012_external_camera.avi</td>
      <td>2759</td>
      <td>speedLimit_1330545914.avi</td>
      <td>44</td>
      <td>26</td>
    </tr>
    <tr>
      <th>12</th>
      <td>./signDatabasePublicFramesOnly/aiua120214-0/fr...</td>
      <td>speedLimit25</td>
      <td>511</td>
      <td>191</td>
      <td>530</td>
      <td>213</td>
      <td>0</td>
      <td>0</td>
      <td>aiua120214-0/DataLog02142012_external_camera.avi</td>
      <td>2766</td>
      <td>speedLimit_1330545914.avi</td>
      <td>51</td>
      <td>26</td>
    </tr>
    <tr>
      <th>13</th>
      <td>./signDatabasePublicFramesOnly/aiua120214-0/fr...</td>
      <td>speedLimit25</td>
      <td>512</td>
      <td>192</td>
      <td>531</td>
      <td>216</td>
      <td>0</td>
      <td>0</td>
      <td>aiua120214-0/DataLog02142012_external_camera.avi</td>
      <td>2773</td>
      <td>speedLimit_1330545914.avi</td>
      <td>58</td>
      <td>26</td>
    </tr>
    <tr>
      <th>14</th>
      <td>./signDatabasePublicFramesOnly/aiua120214-0/fr...</td>
      <td>speedLimit25</td>
      <td>513</td>
      <td>193</td>
      <td>532</td>
      <td>217</td>
      <td>0</td>
      <td>0</td>
      <td>aiua120214-0/DataLog02142012_external_camera.avi</td>
      <td>2780</td>
      <td>speedLimit_1330545914.avi</td>
      <td>65</td>
      <td>26</td>
    </tr>
    <tr>
      <th>15</th>
      <td>./signDatabasePublicFramesOnly/aiua120214-0/fr...</td>
      <td>speedLimit25</td>
      <td>516</td>
      <td>193</td>
      <td>535</td>
      <td>217</td>
      <td>0</td>
      <td>0</td>
      <td>aiua120214-0/DataLog02142012_external_camera.avi</td>
      <td>2787</td>
      <td>speedLimit_1330545914.avi</td>
      <td>72</td>
      <td>26</td>
    </tr>
    <tr>
      <th>16</th>
      <td>./signDatabasePublicFramesOnly/aiua120214-0/fr...</td>
      <td>speedLimit25</td>
      <td>518</td>
      <td>193</td>
      <td>537</td>
      <td>217</td>
      <td>0</td>
      <td>0</td>
      <td>aiua120214-0/DataLog02142012_external_camera.avi</td>
      <td>2794</td>
      <td>speedLimit_1330545914.avi</td>
      <td>79</td>
      <td>26</td>
    </tr>
    <tr>
      <th>17</th>
      <td>./signDatabasePublicFramesOnly/aiua120214-0/fr...</td>
      <td>speedLimit25</td>
      <td>523</td>
      <td>192</td>
      <td>542</td>
      <td>216</td>
      <td>0</td>
      <td>0</td>
      <td>aiua120214-0/DataLog02142012_external_camera.avi</td>
      <td>2801</td>
      <td>speedLimit_1330545914.avi</td>
      <td>86</td>
      <td>26</td>
    </tr>
    <tr>
      <th>18</th>
      <td>./signDatabasePublicFramesOnly/aiua120214-0/fr...</td>
      <td>speedLimit25</td>
      <td>529</td>
      <td>192</td>
      <td>549</td>
      <td>217</td>
      <td>0</td>
      <td>0</td>
      <td>aiua120214-0/DataLog02142012_external_camera.avi</td>
      <td>2808</td>
      <td>speedLimit_1330545914.avi</td>
      <td>93</td>
      <td>26</td>
    </tr>
    <tr>
      <th>19</th>
      <td>./signDatabasePublicFramesOnly/aiua120214-0/fr...</td>
      <td>speedLimit25</td>
      <td>539</td>
      <td>192</td>
      <td>559</td>
      <td>217</td>
      <td>0</td>
      <td>0</td>
      <td>aiua120214-0/DataLog02142012_external_camera.avi</td>
      <td>2815</td>
      <td>speedLimit_1330545914.avi</td>
      <td>100</td>
      <td>26</td>
    </tr>
    <tr>
      <th>20</th>
      <td>./signDatabasePublicFramesOnly/aiua120214-0/fr...</td>
      <td>speedLimit25</td>
      <td>551</td>
      <td>191</td>
      <td>572</td>
      <td>216</td>
      <td>0</td>
      <td>0</td>
      <td>aiua120214-0/DataLog02142012_external_camera.avi</td>
      <td>2822</td>
      <td>speedLimit_1330545914.avi</td>
      <td>107</td>
      <td>26</td>
    </tr>
    <tr>
      <th>21</th>
      <td>./signDatabasePublicFramesOnly/aiua120214-0/fr...</td>
      <td>speedLimit25</td>
      <td>566</td>
      <td>189</td>
      <td>587</td>
      <td>214</td>
      <td>0</td>
      <td>0</td>
      <td>aiua120214-0/DataLog02142012_external_camera.avi</td>
      <td>2829</td>
      <td>speedLimit_1330545914.avi</td>
      <td>114</td>
      <td>26</td>
    </tr>
    <tr>
      <th>22</th>
      <td>./signDatabasePublicFramesOnly/aiua120214-0/fr...</td>
      <td>speedLimit25</td>
      <td>586</td>
      <td>187</td>
      <td>608</td>
      <td>214</td>
      <td>0</td>
      <td>0</td>
      <td>aiua120214-0/DataLog02142012_external_camera.avi</td>
      <td>2836</td>
      <td>speedLimit_1330545914.avi</td>
      <td>121</td>
      <td>26</td>
    </tr>
    <tr>
      <th>23</th>
      <td>./signDatabasePublicFramesOnly/aiua120214-0/fr...</td>
      <td>speedLimit25</td>
      <td>610</td>
      <td>183</td>
      <td>632</td>
      <td>210</td>
      <td>0</td>
      <td>0</td>
      <td>aiua120214-0/DataLog02142012_external_camera.avi</td>
      <td>2843</td>
      <td>speedLimit_1330545914.avi</td>
      <td>128</td>
      <td>26</td>
    </tr>
    <tr>
      <th>24</th>
      <td>./signDatabasePublicFramesOnly/aiua120214-0/fr...</td>
      <td>speedLimit25</td>
      <td>638</td>
      <td>178</td>
      <td>661</td>
      <td>206</td>
      <td>0</td>
      <td>0</td>
      <td>aiua120214-0/DataLog02142012_external_camera.avi</td>
      <td>2850</td>
      <td>speedLimit_1330545914.avi</td>
      <td>135</td>
      <td>26</td>
    </tr>
    <tr>
      <th>25</th>
      <td>./signDatabasePublicFramesOnly/aiua120214-0/fr...</td>
      <td>speedLimit25</td>
      <td>668</td>
      <td>175</td>
      <td>692</td>
      <td>203</td>
      <td>0</td>
      <td>0</td>
      <td>aiua120214-0/DataLog02142012_external_camera.avi</td>
      <td>2857</td>
      <td>speedLimit_1330545914.avi</td>
      <td>142</td>
      <td>26</td>
    </tr>
    <tr>
      <th>26</th>
      <td>./signDatabasePublicFramesOnly/aiua120214-0/fr...</td>
      <td>speedLimit25</td>
      <td>697</td>
      <td>168</td>
      <td>722</td>
      <td>198</td>
      <td>0</td>
      <td>0</td>
      <td>aiua120214-0/DataLog02142012_external_camera.avi</td>
      <td>2864</td>
      <td>speedLimit_1330545914.avi</td>
      <td>149</td>
      <td>26</td>
    </tr>
    <tr>
      <th>27</th>
      <td>./signDatabasePublicFramesOnly/aiua120214-0/fr...</td>
      <td>speedLimit25</td>
      <td>719</td>
      <td>162</td>
      <td>746</td>
      <td>194</td>
      <td>0</td>
      <td>0</td>
      <td>aiua120214-0/DataLog02142012_external_camera.avi</td>
      <td>2871</td>
      <td>speedLimit_1330545914.avi</td>
      <td>156</td>
      <td>26</td>
    </tr>
    <tr>
      <th>28</th>
      <td>./signDatabasePublicFramesOnly/aiua120214-0/fr...</td>
      <td>speedLimit25</td>
      <td>729</td>
      <td>156</td>
      <td>758</td>
      <td>190</td>
      <td>0</td>
      <td>0</td>
      <td>aiua120214-0/DataLog02142012_external_camera.avi</td>
      <td>2878</td>
      <td>speedLimit_1330545914.avi</td>
      <td>163</td>
      <td>26</td>
    </tr>
    <tr>
      <th>29</th>
      <td>./signDatabasePublicFramesOnly/aiua120214-0/fr...</td>
      <td>speedLimit25</td>
      <td>731</td>
      <td>150</td>
      <td>761</td>
      <td>187</td>
      <td>0</td>
      <td>0</td>
      <td>aiua120214-0/DataLog02142012_external_camera.avi</td>
      <td>2885</td>
      <td>speedLimit_1330545914.avi</td>
      <td>170</td>
      <td>26</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>7825</th>
      <td>./signDatabasePublicFramesOnly/vid9/frameAnnot...</td>
      <td>keepRight</td>
      <td>120</td>
      <td>261</td>
      <td>144</td>
      <td>288</td>
      <td>0</td>
      <td>0</td>
      <td>vid9/MVI_0121.MOV</td>
      <td>6807</td>
      <td>keepRight_1324866704.avi</td>
      <td>22</td>
      <td>7</td>
    </tr>
    <tr>
      <th>7826</th>
      <td>./signDatabasePublicFramesOnly/vid9/frameAnnot...</td>
      <td>keepRight</td>
      <td>94</td>
      <td>258</td>
      <td>122</td>
      <td>288</td>
      <td>0</td>
      <td>0</td>
      <td>vid9/MVI_0121.MOV</td>
      <td>6812</td>
      <td>keepRight_1324866704.avi</td>
      <td>27</td>
      <td>7</td>
    </tr>
    <tr>
      <th>7827</th>
      <td>./signDatabasePublicFramesOnly/vid9/frameAnnot...</td>
      <td>keepRight</td>
      <td>59</td>
      <td>259</td>
      <td>91</td>
      <td>295</td>
      <td>0</td>
      <td>0</td>
      <td>vid9/MVI_0121.MOV</td>
      <td>6817</td>
      <td>keepRight_1324866704.avi</td>
      <td>32</td>
      <td>7</td>
    </tr>
    <tr>
      <th>7828</th>
      <td>./signDatabasePublicFramesOnly/vid9/frameAnnot...</td>
      <td>keepRight</td>
      <td>14</td>
      <td>259</td>
      <td>51</td>
      <td>300</td>
      <td>0</td>
      <td>0</td>
      <td>vid9/MVI_0121.MOV</td>
      <td>6822</td>
      <td>keepRight_1324866704.avi</td>
      <td>37</td>
      <td>7</td>
    </tr>
    <tr>
      <th>7829</th>
      <td>./signDatabasePublicFramesOnly/vid9/frameAnnot...</td>
      <td>speedLimit35</td>
      <td>522</td>
      <td>202</td>
      <td>548</td>
      <td>233</td>
      <td>0</td>
      <td>0</td>
      <td>vid9/MVI_0121.MOV</td>
      <td>6927</td>
      <td>speedLimit_1324866719.avi</td>
      <td>2</td>
      <td>28</td>
    </tr>
    <tr>
      <th>7830</th>
      <td>./signDatabasePublicFramesOnly/vid9/frameAnnot...</td>
      <td>speedLimit35</td>
      <td>567</td>
      <td>187</td>
      <td>599</td>
      <td>227</td>
      <td>0</td>
      <td>0</td>
      <td>vid9/MVI_0121.MOV</td>
      <td>6932</td>
      <td>speedLimit_1324866719.avi</td>
      <td>7</td>
      <td>28</td>
    </tr>
    <tr>
      <th>7831</th>
      <td>./signDatabasePublicFramesOnly/vid9/frameAnnot...</td>
      <td>speedLimit35</td>
      <td>162</td>
      <td>232</td>
      <td>177</td>
      <td>250</td>
      <td>0</td>
      <td>0</td>
      <td>vid9/MVI_0121.MOV</td>
      <td>7017</td>
      <td>speedLimit_1324866741.avi</td>
      <td>2</td>
      <td>28</td>
    </tr>
    <tr>
      <th>7832</th>
      <td>./signDatabasePublicFramesOnly/vid9/frameAnnot...</td>
      <td>speedLimit35</td>
      <td>145</td>
      <td>230</td>
      <td>161</td>
      <td>249</td>
      <td>0</td>
      <td>0</td>
      <td>vid9/MVI_0121.MOV</td>
      <td>7022</td>
      <td>speedLimit_1324866741.avi</td>
      <td>7</td>
      <td>28</td>
    </tr>
    <tr>
      <th>7833</th>
      <td>./signDatabasePublicFramesOnly/vid9/frameAnnot...</td>
      <td>speedLimit35</td>
      <td>127</td>
      <td>225</td>
      <td>145</td>
      <td>247</td>
      <td>0</td>
      <td>0</td>
      <td>vid9/MVI_0121.MOV</td>
      <td>7027</td>
      <td>speedLimit_1324866741.avi</td>
      <td>12</td>
      <td>28</td>
    </tr>
    <tr>
      <th>7834</th>
      <td>./signDatabasePublicFramesOnly/vid9/frameAnnot...</td>
      <td>speedLimit35</td>
      <td>103</td>
      <td>216</td>
      <td>123</td>
      <td>240</td>
      <td>0</td>
      <td>0</td>
      <td>vid9/MVI_0121.MOV</td>
      <td>7032</td>
      <td>speedLimit_1324866741.avi</td>
      <td>17</td>
      <td>28</td>
    </tr>
    <tr>
      <th>7835</th>
      <td>./signDatabasePublicFramesOnly/vid9/frameAnnot...</td>
      <td>speedLimit35</td>
      <td>67</td>
      <td>206</td>
      <td>90</td>
      <td>235</td>
      <td>0</td>
      <td>0</td>
      <td>vid9/MVI_0121.MOV</td>
      <td>7037</td>
      <td>speedLimit_1324866741.avi</td>
      <td>22</td>
      <td>28</td>
    </tr>
    <tr>
      <th>7836</th>
      <td>./signDatabasePublicFramesOnly/vid9/frameAnnot...</td>
      <td>speedLimit35</td>
      <td>17</td>
      <td>196</td>
      <td>46</td>
      <td>230</td>
      <td>0</td>
      <td>0</td>
      <td>vid9/MVI_0121.MOV</td>
      <td>7042</td>
      <td>speedLimit_1324866741.avi</td>
      <td>27</td>
      <td>28</td>
    </tr>
    <tr>
      <th>7837</th>
      <td>./signDatabasePublicFramesOnly/vid9/frameAnnot...</td>
      <td>speedLimit35</td>
      <td>225</td>
      <td>227</td>
      <td>239</td>
      <td>242</td>
      <td>0</td>
      <td>0</td>
      <td>vid9/MVI_0121.MOV</td>
      <td>8356</td>
      <td>speedLimit_1324866786.avi</td>
      <td>2</td>
      <td>28</td>
    </tr>
    <tr>
      <th>7838</th>
      <td>./signDatabasePublicFramesOnly/vid9/frameAnnot...</td>
      <td>speedLimit35</td>
      <td>214</td>
      <td>221</td>
      <td>228</td>
      <td>239</td>
      <td>0</td>
      <td>0</td>
      <td>vid9/MVI_0121.MOV</td>
      <td>8361</td>
      <td>speedLimit_1324866786.avi</td>
      <td>7</td>
      <td>28</td>
    </tr>
    <tr>
      <th>7839</th>
      <td>./signDatabasePublicFramesOnly/vid9/frameAnnot...</td>
      <td>speedLimit35</td>
      <td>202</td>
      <td>217</td>
      <td>217</td>
      <td>236</td>
      <td>0</td>
      <td>0</td>
      <td>vid9/MVI_0121.MOV</td>
      <td>8366</td>
      <td>speedLimit_1324866786.avi</td>
      <td>12</td>
      <td>28</td>
    </tr>
    <tr>
      <th>7840</th>
      <td>./signDatabasePublicFramesOnly/vid9/frameAnnot...</td>
      <td>speedLimit35</td>
      <td>187</td>
      <td>214</td>
      <td>204</td>
      <td>235</td>
      <td>0</td>
      <td>0</td>
      <td>vid9/MVI_0121.MOV</td>
      <td>8371</td>
      <td>speedLimit_1324866786.avi</td>
      <td>17</td>
      <td>28</td>
    </tr>
    <tr>
      <th>7841</th>
      <td>./signDatabasePublicFramesOnly/vid9/frameAnnot...</td>
      <td>speedLimit35</td>
      <td>167</td>
      <td>208</td>
      <td>187</td>
      <td>232</td>
      <td>0</td>
      <td>0</td>
      <td>vid9/MVI_0121.MOV</td>
      <td>8376</td>
      <td>speedLimit_1324866786.avi</td>
      <td>22</td>
      <td>28</td>
    </tr>
    <tr>
      <th>7842</th>
      <td>./signDatabasePublicFramesOnly/vid9/frameAnnot...</td>
      <td>speedLimit35</td>
      <td>143</td>
      <td>202</td>
      <td>166</td>
      <td>230</td>
      <td>0</td>
      <td>0</td>
      <td>vid9/MVI_0121.MOV</td>
      <td>8381</td>
      <td>speedLimit_1324866786.avi</td>
      <td>27</td>
      <td>28</td>
    </tr>
    <tr>
      <th>7843</th>
      <td>./signDatabasePublicFramesOnly/vid9/frameAnnot...</td>
      <td>speedLimit35</td>
      <td>117</td>
      <td>192</td>
      <td>141</td>
      <td>223</td>
      <td>0</td>
      <td>0</td>
      <td>vid9/MVI_0121.MOV</td>
      <td>8386</td>
      <td>speedLimit_1324866786.avi</td>
      <td>32</td>
      <td>28</td>
    </tr>
    <tr>
      <th>7844</th>
      <td>./signDatabasePublicFramesOnly/vid9/frameAnnot...</td>
      <td>speedLimit35</td>
      <td>83</td>
      <td>181</td>
      <td>112</td>
      <td>216</td>
      <td>0</td>
      <td>0</td>
      <td>vid9/MVI_0121.MOV</td>
      <td>8391</td>
      <td>speedLimit_1324866786.avi</td>
      <td>37</td>
      <td>28</td>
    </tr>
    <tr>
      <th>7845</th>
      <td>./signDatabasePublicFramesOnly/vid9/frameAnnot...</td>
      <td>speedLimit35</td>
      <td>32</td>
      <td>164</td>
      <td>68</td>
      <td>207</td>
      <td>0</td>
      <td>0</td>
      <td>vid9/MVI_0121.MOV</td>
      <td>8396</td>
      <td>speedLimit_1324866786.avi</td>
      <td>42</td>
      <td>28</td>
    </tr>
    <tr>
      <th>7846</th>
      <td>./signDatabasePublicFramesOnly/vid9/frameAnnot...</td>
      <td>speedLimit35</td>
      <td>149</td>
      <td>232</td>
      <td>164</td>
      <td>251</td>
      <td>0</td>
      <td>0</td>
      <td>vid9/MVI_0121.MOV</td>
      <td>8793</td>
      <td>speedLimit_1324866802.avi</td>
      <td>2</td>
      <td>28</td>
    </tr>
    <tr>
      <th>7847</th>
      <td>./signDatabasePublicFramesOnly/vid9/frameAnnot...</td>
      <td>speedLimit35</td>
      <td>131</td>
      <td>227</td>
      <td>148</td>
      <td>248</td>
      <td>0</td>
      <td>0</td>
      <td>vid9/MVI_0121.MOV</td>
      <td>8798</td>
      <td>speedLimit_1324866802.avi</td>
      <td>7</td>
      <td>28</td>
    </tr>
    <tr>
      <th>7848</th>
      <td>./signDatabasePublicFramesOnly/vid9/frameAnnot...</td>
      <td>speedLimit35</td>
      <td>107</td>
      <td>222</td>
      <td>126</td>
      <td>246</td>
      <td>0</td>
      <td>0</td>
      <td>vid9/MVI_0121.MOV</td>
      <td>8803</td>
      <td>speedLimit_1324866802.avi</td>
      <td>12</td>
      <td>28</td>
    </tr>
    <tr>
      <th>7849</th>
      <td>./signDatabasePublicFramesOnly/vid9/frameAnnot...</td>
      <td>speedLimit35</td>
      <td>76</td>
      <td>216</td>
      <td>96</td>
      <td>242</td>
      <td>0</td>
      <td>0</td>
      <td>vid9/MVI_0121.MOV</td>
      <td>8808</td>
      <td>speedLimit_1324866802.avi</td>
      <td>17</td>
      <td>28</td>
    </tr>
    <tr>
      <th>7850</th>
      <td>./signDatabasePublicFramesOnly/vid9/frameAnnot...</td>
      <td>speedLimit35</td>
      <td>41</td>
      <td>209</td>
      <td>65</td>
      <td>239</td>
      <td>0</td>
      <td>0</td>
      <td>vid9/MVI_0121.MOV</td>
      <td>8813</td>
      <td>speedLimit_1324866802.avi</td>
      <td>22</td>
      <td>28</td>
    </tr>
    <tr>
      <th>7851</th>
      <td>./signDatabasePublicFramesOnly/vid9/frameAnnot...</td>
      <td>speedLimit35</td>
      <td>526</td>
      <td>213</td>
      <td>543</td>
      <td>233</td>
      <td>0</td>
      <td>0</td>
      <td>vid9/MVI_0121.MOV</td>
      <td>8875</td>
      <td>speedLimit_1324866807.avi</td>
      <td>2</td>
      <td>28</td>
    </tr>
    <tr>
      <th>7852</th>
      <td>./signDatabasePublicFramesOnly/vid9/frameAnnot...</td>
      <td>speedLimit35</td>
      <td>546</td>
      <td>208</td>
      <td>564</td>
      <td>230</td>
      <td>0</td>
      <td>0</td>
      <td>vid9/MVI_0121.MOV</td>
      <td>8880</td>
      <td>speedLimit_1324866807.avi</td>
      <td>7</td>
      <td>28</td>
    </tr>
    <tr>
      <th>7853</th>
      <td>./signDatabasePublicFramesOnly/vid9/frameAnnot...</td>
      <td>speedLimit35</td>
      <td>573</td>
      <td>204</td>
      <td>592</td>
      <td>228</td>
      <td>0</td>
      <td>0</td>
      <td>vid9/MVI_0121.MOV</td>
      <td>8885</td>
      <td>speedLimit_1324866807.avi</td>
      <td>12</td>
      <td>28</td>
    </tr>
    <tr>
      <th>7854</th>
      <td>./signDatabasePublicFramesOnly/vid9/frameAnnot...</td>
      <td>speedLimit35</td>
      <td>604</td>
      <td>196</td>
      <td>628</td>
      <td>223</td>
      <td>0</td>
      <td>0</td>
      <td>vid9/MVI_0121.MOV</td>
      <td>8890</td>
      <td>speedLimit_1324866807.avi</td>
      <td>17</td>
      <td>28</td>
    </tr>
  </tbody>
</table>
<p>7855 rows × 13 columns</p>
</div>
</div>

</div>

</div>
</div>

</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[6]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="c1"># Instances per class</span>
<span class="n">instances_per_class</span> <span class="o">=</span> <span class="n">data_frame</span><span class="p">[</span><span class="s2">&quot;Annotation tag&quot;</span><span class="p">]</span><span class="o">.</span><span class="n">value_counts</span><span class="p">()</span>
<span class="nb">print</span><span class="p">(</span><span class="n">instances_per_class</span><span class="p">)</span>
</pre></div>

</div>
</div>
</div>

<div class="output_wrapper">
<div class="output">


<div class="output_area">
<div class="prompt"></div>

<div class="output_subarea output_stream output_stdout output_text">
<pre>stop                      1821
pedestrianCrossing        1085
signalAhead                925
speedLimit35               538
speedLimit25               349
keepRight                  331
addedLane                  294
merge                      266
yield                      236
laneEnds                   210
stopAhead                  168
speedLimit45               141
speedLimit30               140
school                     133
speedLimitUrdbl            132
schoolSpeedLimit25         105
turnRight                   92
rightLaneMustTurn           77
speedLimit65                74
speedLimit40                73
truckSpeedLimit55           60
yieldAhead                  57
roundabout                  53
curveRight                  50
speedLimit50                48
noLeftTurn                  47
curveLeft                   37
dip                         35
slow                        34
turnLeft                    32
rampSpeedAdvisory45         29
noRightTurn                 26
doNotEnter                  23
zoneAhead25                 21
zoneAhead45                 20
thruTrafficMergeLeft        19
rampSpeedAdvisory50         16
speedLimit15                11
rampSpeedAdvisory20         11
doNotPass                    9
thruMergeRight               7
thruMergeLeft                5
rampSpeedAdvisory35          5
rampSpeedAdvisory40          3
rampSpeedAdvisoryUrdbl       3
intersection                 2
speedLimit55                 2
Name: Annotation tag, dtype: int64
</pre>
</div>
</div>

</div>
</div>

</div>
<div class="cell border-box-sizing text_cell rendered">
<div class="prompt input_prompt">
</div>
<div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<h1 id="Data-descriptor-and-keypoint-extraction-for-the-visual-vocabulary">Data descriptor and keypoint extraction for the visual vocabulary<a class="anchor-link" href="#Data-descriptor-and-keypoint-extraction-for-the-visual-vocabulary">&#182;</a></h1>
</div>
</div>
</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[7]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="c1"># Create a CLAHE object for contrast normalization</span>
<span class="c1"># CLAHE (Contrast Limited Adaptive Histogram Equalization)</span>
<span class="n">clahe</span> <span class="o">=</span> <span class="n">cv2</span><span class="o">.</span><span class="n">createCLAHE</span><span class="p">(</span><span class="n">clipLimit</span><span class="o">=</span><span class="mf">4.0</span><span class="p">,</span> <span class="n">tileGridSize</span><span class="o">=</span><span class="p">(</span><span class="mi">8</span><span class="p">,</span><span class="mi">8</span><span class="p">))</span>

<span class="k">def</span> <span class="nf">df_get_kp_desc</span><span class="p">(</span><span class="n">dataframe</span><span class="p">,</span> <span class="n">detector</span><span class="p">):</span>

    <span class="c1"># Define empty arrays to store the information in</span>
    <span class="n">valid_mask</span> <span class="o">=</span> <span class="n">np</span><span class="o">.</span><span class="n">zeros</span><span class="p">((</span><span class="nb">len</span><span class="p">(</span><span class="n">dataframe</span><span class="p">)),</span><span class="n">dtype</span><span class="o">=</span><span class="s1">&#39;bool&#39;</span><span class="p">)</span>
    <span class="n">all_kp</span> <span class="o">=</span> <span class="n">np</span><span class="o">.</span><span class="n">empty</span><span class="p">((</span><span class="nb">len</span><span class="p">(</span><span class="n">dataframe</span><span class="p">)),</span><span class="n">dtype</span><span class="o">=</span><span class="s1">&#39;object&#39;</span><span class="p">)</span>
    <span class="n">all_desc</span> <span class="o">=</span> <span class="n">np</span><span class="o">.</span><span class="n">empty</span><span class="p">((</span><span class="nb">len</span><span class="p">(</span><span class="n">dataframe</span><span class="p">)),</span><span class="n">dtype</span><span class="o">=</span><span class="s1">&#39;object&#39;</span><span class="p">)</span>
    
    <span class="c1"># Here ii is just to keep track</span>
    <span class="n">ii</span> <span class="o">=</span> <span class="mi">0</span>
    <span class="k">for</span> <span class="n">entry</span> <span class="ow">in</span> <span class="n">dataframe</span><span class="o">.</span><span class="n">itertuples</span><span class="p">():</span>
        <span class="c1"># For each entry of the table(row)...</span>
        <span class="c1"># Load the image</span>
        <span class="n">image</span> <span class="o">=</span> <span class="n">cv2</span><span class="o">.</span><span class="n">imread</span><span class="p">(</span><span class="n">entry</span><span class="p">[</span><span class="mi">1</span><span class="p">],</span><span class="mi">0</span><span class="p">)</span>
        
        <span class="c1"># Apply contrast normalization</span>
        <span class="n">imge</span> <span class="o">=</span> <span class="n">clahe</span><span class="o">.</span><span class="n">apply</span><span class="p">(</span><span class="n">image</span><span class="p">)</span>
        
        <span class="c1"># Extract the sign</span>
        <span class="n">sign</span> <span class="o">=</span> <span class="n">image</span><span class="p">[</span><span class="n">entry</span><span class="p">[</span><span class="mi">4</span><span class="p">]:</span><span class="n">entry</span><span class="p">[</span><span class="mi">6</span><span class="p">],</span><span class="n">entry</span><span class="p">[</span><span class="mi">3</span><span class="p">]:</span><span class="n">entry</span><span class="p">[</span><span class="mi">5</span><span class="p">]]</span>
        <span class="c1"># Detect the keypoints in the sign</span>
        <span class="n">kp</span> <span class="o">=</span> <span class="n">detector</span><span class="o">.</span><span class="n">detect</span><span class="p">(</span><span class="n">sign</span><span class="p">)</span>
        
        <span class="c1"># If there is at least 1 keypoint in the sign, the image is valid and we go on to extract the descriptors</span>
        <span class="k">if</span><span class="p">(</span><span class="nb">len</span><span class="p">(</span><span class="n">kp</span><span class="p">)</span> <span class="o">&gt;</span> <span class="mi">0</span><span class="p">):</span>
            <span class="c1"># Set the mask to true</span>
            <span class="n">valid_mask</span><span class="p">[</span><span class="n">ii</span><span class="p">]</span> <span class="o">=</span> <span class="kc">True</span>
            <span class="c1"># Compute the descriptors of the keypoints</span>
            <span class="n">kp</span><span class="p">,</span> <span class="n">desc</span> <span class="o">=</span> <span class="n">detector</span><span class="o">.</span><span class="n">compute</span><span class="p">(</span><span class="n">sign</span><span class="p">,</span> <span class="n">kp</span><span class="p">)</span>
            
            <span class="c1"># Add both kp and desc to its respective arrays</span>
            <span class="n">all_kp</span><span class="p">[</span><span class="n">ii</span><span class="p">]</span> <span class="o">=</span> <span class="n">kp</span>
            <span class="n">all_desc</span><span class="p">[</span><span class="n">ii</span><span class="p">]</span> <span class="o">=</span> <span class="n">desc</span>
        <span class="c1"># Inform about progress every n steps</span>
        <span class="k">if</span><span class="p">(</span> <span class="p">((</span><span class="n">ii</span><span class="o">+</span><span class="mi">1</span><span class="p">)</span> <span class="o">%</span> <span class="mi">250</span><span class="p">)</span> <span class="o">==</span> <span class="mi">0</span><span class="p">):</span>
            <span class="n">T</span> <span class="o">=</span> <span class="n">datetime</span><span class="o">.</span><span class="n">datetime</span><span class="o">.</span><span class="n">now</span><span class="p">()</span>
            <span class="nb">print</span><span class="p">(</span><span class="s2">&quot;Progress-&gt; &quot;</span><span class="p">,</span><span class="nb">str</span><span class="p">(</span><span class="n">ii</span><span class="o">+</span><span class="mi">1</span><span class="p">)</span><span class="o">+</span><span class="s2">&quot;|&quot;</span><span class="o">+</span><span class="nb">str</span><span class="p">(</span><span class="nb">len</span><span class="p">(</span><span class="n">dataframe</span><span class="p">)),</span><span class="s2">&quot;T-&quot;</span><span class="p">,</span><span class="nb">str</span><span class="p">(</span><span class="n">T</span><span class="o">.</span><span class="n">hour</span><span class="p">)</span><span class="o">+</span><span class="s2">&quot;:&quot;</span><span class="p">,</span><span class="nb">str</span><span class="p">(</span><span class="n">T</span><span class="o">.</span><span class="n">minute</span><span class="p">)</span><span class="o">+</span><span class="s2">&quot;:&quot;</span><span class="p">,</span><span class="nb">str</span><span class="p">(</span><span class="n">T</span><span class="o">.</span><span class="n">second</span><span class="p">))</span>
        <span class="n">ii</span><span class="o">+=</span><span class="mi">1</span>
        
    <span class="n">T</span> <span class="o">=</span> <span class="n">datetime</span><span class="o">.</span><span class="n">datetime</span><span class="o">.</span><span class="n">now</span><span class="p">()</span>
    <span class="nb">print</span><span class="p">(</span><span class="s2">&quot;Progress-&gt; &quot;</span><span class="p">,</span><span class="nb">str</span><span class="p">(</span><span class="n">ii</span><span class="p">)</span><span class="o">+</span><span class="s2">&quot;|&quot;</span><span class="o">+</span><span class="nb">str</span><span class="p">(</span><span class="nb">len</span><span class="p">(</span><span class="n">dataframe</span><span class="p">)),</span><span class="s2">&quot;T-&quot;</span><span class="p">,</span><span class="nb">str</span><span class="p">(</span><span class="n">T</span><span class="o">.</span><span class="n">hour</span><span class="p">)</span><span class="o">+</span><span class="s2">&quot;:&quot;</span><span class="p">,</span><span class="nb">str</span><span class="p">(</span><span class="n">T</span><span class="o">.</span><span class="n">minute</span><span class="p">)</span><span class="o">+</span><span class="s2">&quot;:&quot;</span><span class="p">,</span><span class="nb">str</span><span class="p">(</span><span class="n">T</span><span class="o">.</span><span class="n">second</span><span class="p">))</span>

    <span class="k">return</span> <span class="n">all_kp</span><span class="p">,</span> <span class="n">all_desc</span><span class="p">,</span> <span class="n">valid_mask</span>
</pre></div>

</div>
</div>
</div>

</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[8]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="c1"># Create a detector/extractor</span>
<span class="k">if</span><span class="p">(</span><span class="n">detector_choice</span> <span class="o">==</span> <span class="mi">0</span><span class="p">):</span>
    <span class="n">detector</span> <span class="o">=</span> <span class="n">cv2</span><span class="o">.</span><span class="n">KAZE_create</span><span class="p">()</span>
<span class="k">elif</span><span class="p">(</span><span class="n">detector_choice</span> <span class="o">==</span> <span class="mi">1</span><span class="p">):</span>
    <span class="n">detector</span> <span class="o">=</span> <span class="n">cv2</span><span class="o">.</span><span class="n">xfeatures2d</span><span class="o">.</span><span class="n">SURF_create</span><span class="p">()</span>

<span class="c1"># Get the dataframe&#39;s keypoints and descriptors</span>
<span class="o">%</span><span class="k">time</span> kp_vocab, desc_vocab, valid_mask = df_get_kp_desc(df, detector)
</pre></div>

</div>
</div>
</div>

<div class="output_wrapper">
<div class="output">


<div class="output_area">
<div class="prompt"></div>

<div class="output_subarea output_stream output_stdout output_text">
<pre>Progress-&gt;  250|7855 T- 17: 44: 11
Progress-&gt;  500|7855 T- 17: 44: 15
Progress-&gt;  750|7855 T- 17: 44: 19
Progress-&gt;  1000|7855 T- 17: 44: 23
Progress-&gt;  1250|7855 T- 17: 44: 27
Progress-&gt;  1500|7855 T- 17: 44: 31
Progress-&gt;  1750|7855 T- 17: 44: 35
Progress-&gt;  2000|7855 T- 17: 44: 40
Progress-&gt;  2250|7855 T- 17: 44: 44
Progress-&gt;  2500|7855 T- 17: 44: 48
Progress-&gt;  2750|7855 T- 17: 44: 52
Progress-&gt;  3000|7855 T- 17: 44: 56
Progress-&gt;  3250|7855 T- 17: 45: 0
Progress-&gt;  3500|7855 T- 17: 45: 4
Progress-&gt;  3750|7855 T- 17: 45: 8
Progress-&gt;  4000|7855 T- 17: 45: 12
Progress-&gt;  4250|7855 T- 17: 45: 16
Progress-&gt;  4500|7855 T- 17: 45: 20
Progress-&gt;  4750|7855 T- 17: 45: 23
Progress-&gt;  5000|7855 T- 17: 45: 26
Progress-&gt;  5250|7855 T- 17: 45: 30
Progress-&gt;  5500|7855 T- 17: 45: 33
Progress-&gt;  5750|7855 T- 17: 45: 36
Progress-&gt;  6000|7855 T- 17: 45: 40
Progress-&gt;  6250|7855 T- 17: 45: 43
Progress-&gt;  6500|7855 T- 17: 45: 46
Progress-&gt;  6750|7855 T- 17: 45: 49
Progress-&gt;  7000|7855 T- 17: 45: 53
Progress-&gt;  7250|7855 T- 17: 45: 56
Progress-&gt;  7500|7855 T- 17: 45: 59
Progress-&gt;  7750|7855 T- 17: 46: 2
Progress-&gt;  7856|7855 T- 17: 46: 4
CPU times: user 2min 23s, sys: 5.72 s, total: 2min 29s
Wall time: 1min 57s
</pre>
</div>
</div>

</div>
</div>

</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[9]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="c1"># Discard the non-valid instances (those which don&#39;t provide keypoints)</span>
<span class="n">df</span> <span class="o">=</span> <span class="n">df</span><span class="p">[</span><span class="n">valid_mask</span><span class="p">]</span>
<span class="n">kp_vocab</span> <span class="o">=</span> <span class="n">kp_vocab</span><span class="p">[</span><span class="n">valid_mask</span><span class="p">]</span>
<span class="n">desc_vocab</span> <span class="o">=</span> <span class="n">desc_vocab</span><span class="p">[</span><span class="n">valid_mask</span><span class="p">]</span>
<span class="c1"># To avoid cutting down more than once</span>
<span class="n">valid_mask</span> <span class="o">=</span> <span class="n">valid_mask</span><span class="p">[</span><span class="n">valid_mask</span><span class="p">]</span>
</pre></div>

</div>
</div>
</div>

</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[10]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="c1"># Instances per class</span>
<span class="nb">print</span><span class="p">(</span><span class="s2">&quot;Valid instances:&quot;</span><span class="p">,</span><span class="nb">len</span><span class="p">(</span><span class="n">df</span><span class="p">))</span>
<span class="nb">print</span><span class="p">(</span><span class="s2">&quot;Categories:&quot;</span><span class="p">,</span><span class="nb">len</span><span class="p">(</span><span class="n">df</span><span class="p">[</span><span class="s2">&quot;Annotation tag&quot;</span><span class="p">]</span><span class="o">.</span><span class="n">value_counts</span><span class="p">()))</span>
<span class="nb">print</span><span class="p">(</span><span class="s2">&quot;</span><span class="se">\n</span><span class="s2">Instances of each category</span><span class="se">\n</span><span class="s2">&quot;</span><span class="p">,</span><span class="n">df</span><span class="p">[</span><span class="s2">&quot;Annotation tag&quot;</span><span class="p">]</span><span class="o">.</span><span class="n">value_counts</span><span class="p">())</span>
</pre></div>

</div>
</div>
</div>

<div class="output_wrapper">
<div class="output">


<div class="output_area">
<div class="prompt"></div>

<div class="output_subarea output_stream output_stdout output_text">
<pre>Valid instances: 7062
Categories: 45

Instances of each category
 stop                    1556
pedestrianCrossing      1053
signalAhead              841
speedLimit35             425
keepRight                307
speedLimit25             296
addedLane                273
merge                    243
yield                    218
stopAhead                168
laneEnds                 159
speedLimit45             141
school                   124
speedLimit30             123
schoolSpeedLimit25       104
speedLimitUrdbl           96
turnRight                 91
speedLimit40              73
speedLimit65              71
rightLaneMustTurn         70
yieldAhead                55
roundabout                53
truckSpeedLimit55         50
speedLimit50              47
noLeftTurn                47
curveRight                44
curveLeft                 37
dip                       35
slow                      34
turnLeft                  32
noRightTurn               26
rampSpeedAdvisory45       25
zoneAhead25               21
doNotEnter                21
zoneAhead45               20
thruTrafficMergeLeft      19
rampSpeedAdvisory50       14
speedLimit15              11
doNotPass                  9
rampSpeedAdvisory20        9
thruMergeRight             7
thruMergeLeft              5
rampSpeedAdvisory35        5
rampSpeedAdvisory40        2
speedLimit55               2
Name: Annotation tag, dtype: int64
</pre>
</div>
</div>

</div>
</div>

</div>
<div class="cell border-box-sizing text_cell rendered">
<div class="prompt input_prompt">
</div>
<div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<h1 id="Drop-categories-with-few-instances-(&lt;-N=10)">Drop categories with few instances (&lt; N=10)<a class="anchor-link" href="#Drop-categories-with-few-instances-(&lt;-N=10)">&#182;</a></h1>
</div>
</div>
</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[11]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="nb">print</span><span class="p">(</span><span class="n">df</span><span class="p">[</span><span class="s2">&quot;Annotation tag&quot;</span><span class="p">]</span><span class="o">.</span><span class="n">value_counts</span><span class="p">()[</span><span class="n">df</span><span class="p">[</span><span class="s2">&quot;Annotation tag&quot;</span><span class="p">]</span><span class="o">.</span><span class="n">value_counts</span><span class="p">()</span><span class="o">&lt;</span><span class="mi">10</span><span class="p">])</span>
</pre></div>

</div>
</div>
</div>

<div class="output_wrapper">
<div class="output">


<div class="output_area">
<div class="prompt"></div>

<div class="output_subarea output_stream output_stdout output_text">
<pre>doNotPass              9
rampSpeedAdvisory20    9
thruMergeRight         7
thruMergeLeft          5
rampSpeedAdvisory35    5
rampSpeedAdvisory40    2
speedLimit55           2
Name: Annotation tag, dtype: int64
</pre>
</div>
</div>

</div>
</div>

</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[12]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="c1"># Group all the data labels</span>
<span class="n">data_labels</span> <span class="o">=</span> <span class="n">np</span><span class="o">.</span><span class="n">int32</span><span class="p">(</span><span class="n">df</span><span class="p">[</span><span class="s2">&quot;label&quot;</span><span class="p">]</span><span class="o">.</span><span class="n">values</span><span class="p">)</span>

<span class="c1"># Find which labels have less than N examples in the dataset</span>
<span class="n">n_drop_cutout</span><span class="o">=</span><span class="mi">10</span>
<span class="n">mask_to_drop</span> <span class="o">=</span> <span class="n">df</span><span class="p">[</span><span class="s2">&quot;label&quot;</span><span class="p">]</span><span class="o">.</span><span class="n">value_counts</span><span class="p">()</span> <span class="o">&lt;</span> <span class="n">n_drop_cutout</span>
<span class="n">labels_to_drop</span> <span class="o">=</span> <span class="n">mask_to_drop</span><span class="o">.</span><span class="n">index</span><span class="p">[</span><span class="n">mask_to_drop</span><span class="o">.</span><span class="n">values</span><span class="p">]</span>
<span class="n">labels_to_drop</span> <span class="o">=</span> <span class="n">np</span><span class="o">.</span><span class="n">array</span><span class="p">(</span><span class="n">labels_to_drop</span><span class="p">,</span><span class="n">dtype</span><span class="o">=</span><span class="s1">&#39;int32&#39;</span><span class="p">)</span>

<span class="n">mask_to_keep</span> <span class="o">=</span> <span class="n">np</span><span class="o">.</span><span class="n">ones</span><span class="p">(</span><span class="nb">len</span><span class="p">(</span><span class="n">data_labels</span><span class="p">),</span><span class="n">dtype</span><span class="o">=</span><span class="s1">&#39;bool&#39;</span><span class="p">)</span>

<span class="k">for</span> <span class="n">ii</span> <span class="ow">in</span> <span class="nb">range</span><span class="p">(</span><span class="nb">len</span><span class="p">(</span><span class="n">data_labels</span><span class="p">)):</span>
    <span class="k">if</span><span class="p">(</span><span class="n">data_labels</span><span class="p">[</span><span class="n">ii</span><span class="p">]</span> <span class="ow">in</span> <span class="n">labels_to_drop</span><span class="p">):</span>
        <span class="n">mask_to_keep</span><span class="p">[</span><span class="n">ii</span><span class="p">]</span> <span class="o">=</span> <span class="kc">False</span>

<span class="n">df</span> <span class="o">=</span> <span class="n">df</span><span class="p">[</span><span class="n">mask_to_keep</span><span class="p">]</span>
<span class="n">kp_vocab</span> <span class="o">=</span> <span class="n">kp_vocab</span><span class="p">[</span><span class="n">mask_to_keep</span><span class="p">]</span>
<span class="n">desc_vocab</span> <span class="o">=</span> <span class="n">desc_vocab</span><span class="p">[</span><span class="n">mask_to_keep</span><span class="p">]</span>
</pre></div>

</div>
</div>
</div>

</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[13]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="nb">print</span><span class="p">(</span><span class="s2">&quot;New size:&quot;</span><span class="p">,</span> <span class="nb">len</span><span class="p">(</span><span class="n">df</span><span class="p">),</span><span class="s2">&quot;- dropped&quot;</span><span class="p">,</span> <span class="nb">len</span><span class="p">(</span><span class="n">mask_to_keep</span><span class="p">[</span><span class="o">~</span><span class="n">mask_to_keep</span><span class="p">]))</span>
</pre></div>

</div>
</div>
</div>

<div class="output_wrapper">
<div class="output">


<div class="output_area">
<div class="prompt"></div>

<div class="output_subarea output_stream output_stdout output_text">
<pre>New size: 7023 - dropped 39
</pre>
</div>
</div>

</div>
</div>

</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[14]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="c1"># Descriptor count</span>
<span class="k">for</span> <span class="n">desc</span> <span class="ow">in</span> <span class="n">desc_vocab</span><span class="p">:</span>
    
</pre></div>

</div>
</div>
</div>

<div class="output_wrapper">
<div class="output">


<div class="output_area">
<div class="prompt"></div>

<div class="output_subarea output_stream output_stdout output_text">
<pre>Series([], Name: Annotation tag, dtype: int64)
</pre>
</div>
</div>

</div>
</div>

</div>
<div class="cell border-box-sizing text_cell rendered">
<div class="prompt input_prompt">
</div>
<div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<h1 id="Vocabulary-extraction-from-the-computed-descriptors">Vocabulary extraction from the computed descriptors<a class="anchor-link" href="#Vocabulary-extraction-from-the-computed-descriptors">&#182;</a></h1>
</div>
</div>
</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[15]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="k">def</span> <span class="nf">get_vocabulary</span><span class="p">(</span><span class="n">descriptors</span><span class="p">,</span> <span class="n">K</span><span class="p">):</span>
    <span class="c1"># Define a BOW k-means trainer with K centroids</span>
    <span class="n">bow_trainer</span> <span class="o">=</span> <span class="n">cv2</span><span class="o">.</span><span class="n">BOWKMeansTrainer</span><span class="p">(</span><span class="n">K</span><span class="p">)</span>
    
    <span class="c1"># Add each descriptor</span>
    <span class="n">dcount</span><span class="o">=</span><span class="mi">0</span>
    <span class="k">for</span> <span class="n">desc</span> <span class="ow">in</span> <span class="n">descriptors</span><span class="p">:</span>
        <span class="n">dcount</span><span class="o">+=</span><span class="n">desc</span><span class="o">.</span><span class="n">shape</span><span class="p">[</span><span class="mi">0</span><span class="p">]</span>
        <span class="n">bow_trainer</span><span class="o">.</span><span class="n">add</span><span class="p">(</span><span class="n">np</span><span class="o">.</span><span class="n">float32</span><span class="p">(</span><span class="n">desc</span><span class="p">))</span>
        
    

    <span class="c1"># Cluster the descriptors into K clusters using the trainer</span>
    <span class="k">return</span> <span class="n">bow_trainer</span><span class="o">.</span><span class="n">cluster</span><span class="p">()</span>
</pre></div>

</div>
</div>
</div>

</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[16]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="c1"># Compute the visual vocabulary</span>
<span class="n">n_centroids</span> <span class="o">=</span> <span class="mi">1024</span>
<span class="o">%</span><span class="k">time</span> visual_vocabulary = get_vocabulary(desc_vocab, n_centroids)
</pre></div>

</div>
</div>
</div>

<div class="output_wrapper">
<div class="output">


<div class="output_area">
<div class="prompt"></div>

<div class="output_subarea output_stream output_stdout output_text">
<pre>CPU times: user 25min 22s, sys: 4.13 s, total: 25min 26s
Wall time: 6min 55s
</pre>
</div>
</div>

</div>
</div>

</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[17]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="nb">print</span><span class="p">(</span><span class="n">visual_vocabulary</span><span class="o">.</span><span class="n">shape</span><span class="p">)</span>
</pre></div>

</div>
</div>
</div>

<div class="output_wrapper">
<div class="output">


<div class="output_area">
<div class="prompt"></div>

<div class="output_subarea output_stream output_stdout output_text">
<pre>(1024, 64)
</pre>
</div>
</div>

</div>
</div>

</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[18]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="c1"># Define a matcher for the BOW extractor, in addition to the detector we already had</span>
<span class="n">matcher</span> <span class="o">=</span> <span class="n">cv2</span><span class="o">.</span><span class="n">FlannBasedMatcher_create</span><span class="p">()</span>
<span class="n">bow</span> <span class="o">=</span> <span class="n">cv2</span><span class="o">.</span><span class="n">BOWImgDescriptorExtractor</span><span class="p">(</span><span class="n">detector</span><span class="p">,</span> <span class="n">matcher</span><span class="p">)</span>
<span class="n">bow</span><span class="o">.</span><span class="n">setVocabulary</span><span class="p">(</span><span class="n">visual_vocabulary</span><span class="p">)</span>
</pre></div>

</div>
</div>
</div>

</div>
<div class="cell border-box-sizing text_cell rendered">
<div class="prompt input_prompt">
</div>
<div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<h1 id="Secuential-word-histogram-extraction-(auxiliary-to-parallel's)">Secuential word histogram extraction (auxiliary to parallel's)<a class="anchor-link" href="#Secuential-word-histogram-extraction-(auxiliary-to-parallel's)">&#182;</a></h1>
</div>
</div>
</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[19]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="k">def</span> <span class="nf">histograms_to_array</span><span class="p">(</span><span class="n">histograms</span><span class="p">):</span>
    <span class="c1"># Right now, the histogram data is an &#39;Object&#39; array. We need to convert it to a float32 matrix</span>
    <span class="c1"># Define a new float array</span>
    <span class="n">hist_array</span> <span class="o">=</span> <span class="n">np</span><span class="o">.</span><span class="n">zeros</span><span class="p">((</span><span class="n">histograms</span><span class="o">.</span><span class="n">shape</span><span class="p">[</span><span class="mi">0</span><span class="p">],</span><span class="n">histograms</span><span class="p">[</span><span class="mi">0</span><span class="p">]</span><span class="o">.</span><span class="n">shape</span><span class="p">[</span><span class="mi">1</span><span class="p">]),</span><span class="n">dtype</span><span class="o">=</span><span class="s1">&#39;float32&#39;</span><span class="p">)</span>
    <span class="c1"># Copy each row correctly</span>
    <span class="k">for</span> <span class="n">ii</span> <span class="ow">in</span> <span class="nb">range</span><span class="p">(</span><span class="nb">len</span><span class="p">(</span><span class="n">histograms</span><span class="p">)):</span>
        <span class="n">hist_array</span><span class="p">[</span><span class="n">ii</span><span class="p">]</span> <span class="o">=</span> <span class="n">histograms</span><span class="p">[</span><span class="n">ii</span><span class="p">][</span><span class="mi">0</span><span class="p">]</span>
    
    <span class="k">return</span> <span class="n">hist_array</span>
</pre></div>

</div>
</div>
</div>

</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[20]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="k">def</span> <span class="nf">get_word_histogram</span><span class="p">(</span><span class="n">input_image</span><span class="p">,</span> <span class="n">bow_extractor</span><span class="p">,</span> <span class="n">detector</span><span class="p">):</span>
    <span class="c1"># Extract keypoints and descriptors for the image</span>
    <span class="n">kp</span> <span class="o">=</span> <span class="n">detector</span><span class="o">.</span><span class="n">detect</span><span class="p">(</span><span class="n">input_image</span><span class="p">,</span> <span class="kc">None</span><span class="p">)</span>

    <span class="k">return</span> <span class="n">bow_extractor</span><span class="o">.</span><span class="n">compute</span><span class="p">(</span><span class="n">input_image</span><span class="p">,</span> <span class="n">kp</span><span class="p">)</span>
</pre></div>

</div>
</div>
</div>

</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[21]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="c1"># Computes the histograms given a bag of words and a dataframe</span>
<span class="k">def</span> <span class="nf">df_compute_histograms</span><span class="p">(</span><span class="n">dataframe</span><span class="p">,</span> <span class="n">bow_extractor</span><span class="p">,</span> <span class="n">detector</span><span class="p">):</span>
    
    <span class="n">histograms</span> <span class="o">=</span> <span class="n">np</span><span class="o">.</span><span class="n">empty</span><span class="p">((</span><span class="nb">len</span><span class="p">(</span><span class="n">dataframe</span><span class="p">)),</span><span class="n">dtype</span><span class="o">=</span><span class="s1">&#39;object&#39;</span><span class="p">)</span>
    <span class="n">time</span> <span class="o">=</span> <span class="n">datetime</span><span class="o">.</span><span class="n">datetime</span><span class="o">.</span><span class="n">now</span><span class="p">()</span>
    
    <span class="n">ii</span> <span class="o">=</span> <span class="mi">0</span>
    <span class="k">for</span> <span class="n">entry</span> <span class="ow">in</span> <span class="n">dataframe</span><span class="o">.</span><span class="n">itertuples</span><span class="p">():</span>
        <span class="c1"># For each entry of the table(row)...</span>
        <span class="c1"># Load the image</span>
        <span class="n">image</span> <span class="o">=</span> <span class="n">cv2</span><span class="o">.</span><span class="n">imread</span><span class="p">(</span><span class="n">entry</span><span class="p">[</span><span class="mi">1</span><span class="p">],</span><span class="mi">0</span><span class="p">)</span>

        <span class="c1"># Apply contrast normalization</span>
        <span class="n">imge</span> <span class="o">=</span> <span class="n">clahe</span><span class="o">.</span><span class="n">apply</span><span class="p">(</span><span class="n">image</span><span class="p">)</span>
                
        <span class="c1"># Extract the word histogram</span>
        <span class="n">hist</span> <span class="o">=</span> <span class="n">get_word_histogram</span><span class="p">(</span><span class="n">image</span><span class="p">,</span> <span class="n">bow_extractor</span><span class="p">,</span> <span class="n">detector</span><span class="p">)</span>
        <span class="n">histograms</span><span class="p">[</span><span class="n">ii</span><span class="p">]</span> <span class="o">=</span> <span class="n">hist</span>

        <span class="c1"># Inform about progress every n steps</span>
        <span class="k">if</span><span class="p">(</span> <span class="p">((</span><span class="n">ii</span><span class="o">+</span><span class="mi">1</span><span class="p">)</span> <span class="o">%</span> <span class="mi">100</span><span class="p">)</span> <span class="o">==</span> <span class="mi">0</span><span class="p">):</span>
            <span class="n">T</span> <span class="o">=</span> <span class="n">datetime</span><span class="o">.</span><span class="n">datetime</span><span class="o">.</span><span class="n">now</span><span class="p">()</span>
            <span class="nb">print</span><span class="p">(</span><span class="s2">&quot;Thread [&quot;</span><span class="o">+</span> <span class="nb">str</span><span class="p">(</span> <span class="n">threading</span><span class="o">.</span><span class="n">current_thread</span><span class="p">()</span> <span class="p">)</span> <span class="o">+</span><span class="s2">&quot;] Progress-&gt; &quot;</span><span class="p">,</span><span class="nb">str</span><span class="p">(</span><span class="n">ii</span><span class="o">+</span><span class="mi">1</span><span class="p">)</span><span class="o">+</span><span class="s2">&quot;|&quot;</span><span class="o">+</span><span class="nb">str</span><span class="p">(</span><span class="nb">len</span><span class="p">(</span><span class="n">dataframe</span><span class="p">)),</span><span class="s2">&quot;T-&quot;</span><span class="p">,</span><span class="nb">str</span><span class="p">(</span><span class="n">T</span><span class="o">.</span><span class="n">hour</span><span class="p">)</span><span class="o">+</span><span class="s2">&quot;:&quot;</span><span class="p">,</span><span class="nb">str</span><span class="p">(</span><span class="n">T</span><span class="o">.</span><span class="n">minute</span><span class="p">)</span><span class="o">+</span><span class="s2">&quot;:&quot;</span><span class="p">,</span><span class="nb">str</span><span class="p">(</span><span class="n">T</span><span class="o">.</span><span class="n">second</span><span class="p">))</span>
        <span class="n">ii</span><span class="o">+=</span><span class="mi">1</span>
        
    <span class="n">T</span> <span class="o">=</span> <span class="n">datetime</span><span class="o">.</span><span class="n">datetime</span><span class="o">.</span><span class="n">now</span><span class="p">()</span>
    <span class="nb">print</span><span class="p">(</span><span class="s2">&quot;Progress-&gt; &quot;</span><span class="p">,</span><span class="nb">str</span><span class="p">(</span><span class="n">ii</span><span class="p">)</span><span class="o">+</span><span class="s2">&quot;|&quot;</span><span class="o">+</span><span class="nb">str</span><span class="p">(</span><span class="nb">len</span><span class="p">(</span><span class="n">dataframe</span><span class="p">)),</span><span class="s2">&quot;T-&quot;</span><span class="p">,</span><span class="nb">str</span><span class="p">(</span><span class="n">T</span><span class="o">.</span><span class="n">hour</span><span class="p">)</span><span class="o">+</span><span class="s2">&quot;:&quot;</span><span class="p">,</span><span class="nb">str</span><span class="p">(</span><span class="n">T</span><span class="o">.</span><span class="n">minute</span><span class="p">)</span><span class="o">+</span><span class="s2">&quot;:&quot;</span><span class="p">,</span><span class="nb">str</span><span class="p">(</span><span class="n">T</span><span class="o">.</span><span class="n">second</span><span class="p">))</span>
    <span class="k">return</span> <span class="n">histograms_to_array</span><span class="p">(</span><span class="n">histograms</span><span class="p">)</span>
</pre></div>

</div>
</div>
</div>

</div>
<div class="cell border-box-sizing text_cell rendered">
<div class="prompt input_prompt">
</div>
<div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<h1 id="Parallel-word-histogram-extraction">Parallel word histogram extraction<a class="anchor-link" href="#Parallel-word-histogram-extraction">&#182;</a></h1>
</div>
</div>
</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[22]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="c1">####################</span>
<span class="c1"># Preferred method #</span>
<span class="c1">####################</span>

<span class="c1"># Global variables for the detector and bag of words</span>
<span class="n">parallel_det</span> <span class="o">=</span> <span class="n">detector</span>
<span class="n">parallel_bow</span> <span class="o">=</span> <span class="n">bow</span>

<span class="c1"># Auxiliary function to process every entry</span>
<span class="k">def</span> <span class="nf">process_slice</span><span class="p">(</span><span class="n">df_slice</span><span class="p">):</span>
    <span class="k">return</span> <span class="n">df_compute_histograms</span><span class="p">(</span><span class="n">df_slice</span><span class="p">,</span> <span class="n">parallel_bow</span><span class="p">,</span> <span class="n">parallel_det</span><span class="p">)</span>

<span class="c1"># Parallel computing of the dataframe&#39;s word representation using the bow parallel_bow and the detector parallel_det</span>
<span class="k">def</span> <span class="nf">df_compute_histograms_parallel</span><span class="p">(</span><span class="n">dataframe</span><span class="p">,</span> <span class="n">n_jobs</span> <span class="o">=</span> <span class="n">joblib</span><span class="o">.</span><span class="n">cpu_count</span><span class="p">()):</span>
    
    <span class="c1"># Slice the dataframe in n_jobs</span>
    <span class="n">df_slices</span> <span class="o">=</span> <span class="n">np</span><span class="o">.</span><span class="n">empty</span><span class="p">(</span><span class="n">n_jobs</span><span class="p">,</span><span class="n">dtype</span><span class="o">=</span><span class="s1">&#39;object&#39;</span><span class="p">)</span>
    
    <span class="c1"># The last thread will have n_jobs-1 jobs less (3 less in case of 4 cores)</span>
    <span class="n">slice_len</span> <span class="o">=</span> <span class="mi">1</span><span class="o">+</span><span class="nb">int</span><span class="p">((</span><span class="nb">len</span><span class="p">(</span><span class="n">dataframe</span><span class="p">)</span><span class="o">/</span><span class="n">n_jobs</span><span class="p">))</span>
    <span class="nb">print</span><span class="p">(</span><span class="s2">&quot;Slice: &quot;</span><span class="p">,</span><span class="n">slice_len</span><span class="p">)</span>
    <span class="k">for</span> <span class="n">ii</span> <span class="ow">in</span> <span class="nb">range</span><span class="p">(</span><span class="n">n_jobs</span><span class="o">-</span><span class="mi">1</span><span class="p">):</span>
        <span class="n">df_slices</span><span class="p">[</span><span class="n">ii</span><span class="p">]</span> <span class="o">=</span> <span class="n">dataframe</span><span class="o">.</span><span class="n">iloc</span><span class="p">[(</span><span class="n">ii</span><span class="o">*</span><span class="n">slice_len</span><span class="p">)</span> <span class="p">:</span> <span class="p">((</span><span class="n">ii</span><span class="o">+</span><span class="mi">1</span><span class="p">)</span><span class="o">*</span><span class="n">slice_len</span><span class="p">)]</span>
    <span class="c1"># Last slice, from the previous to the end</span>
    <span class="n">df_slices</span><span class="p">[</span><span class="n">n_jobs</span><span class="o">-</span><span class="mi">1</span><span class="p">]</span> <span class="o">=</span> <span class="n">dataframe</span><span class="o">.</span><span class="n">iloc</span><span class="p">[((</span><span class="n">n_jobs</span><span class="o">-</span><span class="mi">1</span><span class="p">)</span><span class="o">*</span><span class="n">slice_len</span><span class="p">)</span> <span class="p">:</span> <span class="p">]</span>

    
    <span class="c1"># Show the beginning of the extraction</span>
    <span class="n">T</span> <span class="o">=</span> <span class="n">datetime</span><span class="o">.</span><span class="n">datetime</span><span class="o">.</span><span class="n">now</span><span class="p">()</span>
    <span class="nb">print</span><span class="p">(</span><span class="s2">&quot;Progress(Start)-&gt; &quot;</span><span class="p">,</span><span class="s2">&quot;0&quot;</span><span class="o">+</span><span class="s2">&quot;|&quot;</span><span class="p">,</span><span class="nb">str</span><span class="p">(</span><span class="nb">len</span><span class="p">(</span><span class="n">dataframe</span><span class="p">)),</span><span class="s2">&quot;T-&quot;</span><span class="p">,</span><span class="nb">str</span><span class="p">(</span><span class="n">T</span><span class="o">.</span><span class="n">hour</span><span class="p">)</span><span class="o">+</span><span class="s2">&quot;:&quot;</span><span class="p">,</span><span class="nb">str</span><span class="p">(</span><span class="n">T</span><span class="o">.</span><span class="n">minute</span><span class="p">)</span><span class="o">+</span><span class="s2">&quot;:&quot;</span><span class="p">,</span><span class="nb">str</span><span class="p">(</span><span class="n">T</span><span class="o">.</span><span class="n">second</span><span class="p">))</span>
    
    <span class="c1"># Run the function process_input in parallel over the file names (index for time measurement)</span>
    <span class="n">res</span> <span class="o">=</span> <span class="n">joblib</span><span class="o">.</span><span class="n">Parallel</span><span class="p">(</span><span class="n">n_jobs</span><span class="o">=</span><span class="n">joblib</span><span class="o">.</span><span class="n">cpu_count</span><span class="p">(),</span> <span class="n">backend</span><span class="o">=</span><span class="s2">&quot;threading&quot;</span><span class="p">,</span><span class="n">verbose</span><span class="o">=</span><span class="mi">1</span><span class="p">)(</span><span class="n">joblib</span><span class="o">.</span><span class="n">delayed</span><span class="p">(</span><span class="n">process_slice</span><span class="p">)(</span><span class="n">slc</span><span class="p">)</span> <span class="k">for</span> <span class="n">slc</span> <span class="ow">in</span> <span class="n">df_slices</span><span class="p">)</span>
                                    
    <span class="n">res</span> <span class="o">=</span> <span class="n">np</span><span class="o">.</span><span class="n">array</span><span class="p">(</span><span class="n">res</span><span class="p">)</span>
    <span class="c1"># Unpack the values</span>
    <span class="n">res</span> <span class="o">=</span> <span class="n">np</span><span class="o">.</span><span class="n">vstack</span><span class="p">(</span><span class="n">res</span><span class="p">[:])</span>
    
    <span class="n">T</span> <span class="o">=</span> <span class="n">datetime</span><span class="o">.</span><span class="n">datetime</span><span class="o">.</span><span class="n">now</span><span class="p">()</span>
    <span class="nb">print</span><span class="p">(</span><span class="s2">&quot;Progress-&gt; &quot;</span><span class="p">,</span><span class="nb">str</span><span class="p">(</span><span class="nb">len</span><span class="p">(</span><span class="n">dataframe</span><span class="p">))</span><span class="o">+</span><span class="s2">&quot;|&quot;</span><span class="o">+</span><span class="nb">str</span><span class="p">(</span><span class="nb">len</span><span class="p">(</span><span class="n">dataframe</span><span class="p">)),</span><span class="s2">&quot;T-&quot;</span><span class="p">,</span><span class="nb">str</span><span class="p">(</span><span class="n">T</span><span class="o">.</span><span class="n">hour</span><span class="p">)</span><span class="o">+</span><span class="s2">&quot;:&quot;</span><span class="p">,</span><span class="nb">str</span><span class="p">(</span><span class="n">T</span><span class="o">.</span><span class="n">minute</span><span class="p">)</span><span class="o">+</span><span class="s2">&quot;:&quot;</span><span class="p">,</span><span class="nb">str</span><span class="p">(</span><span class="n">T</span><span class="o">.</span><span class="n">second</span><span class="p">))</span>
    
    <span class="k">return</span> <span class="n">res</span>
</pre></div>

</div>
</div>
</div>

</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[&nbsp;]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span> 
</pre></div>

</div>
</div>
</div>

</div>
<div class="cell border-box-sizing text_cell rendered">
<div class="prompt input_prompt">
</div>
<div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<h1 id="Extract-image-data-using-BOW-descriptors">Extract image data using BOW descriptors<a class="anchor-link" href="#Extract-image-data-using-BOW-descriptors">&#182;</a></h1>
</div>
</div>
</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[23]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="n">data_labels</span> <span class="o">=</span> <span class="n">np</span><span class="o">.</span><span class="n">int32</span><span class="p">(</span><span class="n">df</span><span class="p">[</span><span class="s2">&quot;label&quot;</span><span class="p">]</span><span class="o">.</span><span class="n">values</span><span class="p">)</span>
</pre></div>

</div>
</div>
</div>

</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[24]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="o">%</span><span class="k">time</span> data_histograms = df_compute_histograms_parallel(df)
</pre></div>

</div>
</div>
</div>

<div class="output_wrapper">
<div class="output">


<div class="output_area">
<div class="prompt"></div>

<div class="output_subarea output_stream output_stdout output_text">
<pre>Slice:  1756
Progress(Start)-&gt;  0| 7023 T- 17: 52: 59
Thread [&lt;DummyProcess(Thread-7, started daemon 140515332835072)&gt;] Progress-&gt;  100|1755 T- 17: 55: 1
Thread [&lt;DummyProcess(Thread-6, started daemon 140515341227776)&gt;] Progress-&gt;  100|1756 T- 17: 55: 39
Thread [&lt;DummyProcess(Thread-4, started daemon 140515358013184)&gt;] Progress-&gt;  100|1756 T- 17: 56: 10
Thread [&lt;DummyProcess(Thread-5, started daemon 140515349620480)&gt;] Progress-&gt;  100|1756 T- 17: 56: 27
Thread [&lt;DummyProcess(Thread-7, started daemon 140515332835072)&gt;] Progress-&gt;  200|1755 T- 17: 57: 2
Thread [&lt;DummyProcess(Thread-6, started daemon 140515341227776)&gt;] Progress-&gt;  200|1756 T- 17: 58: 26
Thread [&lt;DummyProcess(Thread-7, started daemon 140515332835072)&gt;] Progress-&gt;  300|1755 T- 17: 59: 7
Thread [&lt;DummyProcess(Thread-4, started daemon 140515358013184)&gt;] Progress-&gt;  200|1756 T- 17: 59: 19
Thread [&lt;DummyProcess(Thread-5, started daemon 140515349620480)&gt;] Progress-&gt;  200|1756 T- 17: 59: 50
Thread [&lt;DummyProcess(Thread-6, started daemon 140515341227776)&gt;] Progress-&gt;  300|1756 T- 18: 1: 1
Thread [&lt;DummyProcess(Thread-7, started daemon 140515332835072)&gt;] Progress-&gt;  400|1755 T- 18: 1: 14
Thread [&lt;DummyProcess(Thread-4, started daemon 140515358013184)&gt;] Progress-&gt;  300|1756 T- 18: 2: 25
Thread [&lt;DummyProcess(Thread-7, started daemon 140515332835072)&gt;] Progress-&gt;  500|1755 T- 18: 3: 13
Thread [&lt;DummyProcess(Thread-5, started daemon 140515349620480)&gt;] Progress-&gt;  300|1756 T- 18: 3: 16
Thread [&lt;DummyProcess(Thread-6, started daemon 140515341227776)&gt;] Progress-&gt;  400|1756 T- 18: 3: 37
Thread [&lt;DummyProcess(Thread-7, started daemon 140515332835072)&gt;] Progress-&gt;  600|1755 T- 18: 5: 7
Thread [&lt;DummyProcess(Thread-4, started daemon 140515358013184)&gt;] Progress-&gt;  400|1756 T- 18: 5: 37
Thread [&lt;DummyProcess(Thread-6, started daemon 140515341227776)&gt;] Progress-&gt;  500|1756 T- 18: 6: 20
Thread [&lt;DummyProcess(Thread-5, started daemon 140515349620480)&gt;] Progress-&gt;  400|1756 T- 18: 6: 38
Thread [&lt;DummyProcess(Thread-7, started daemon 140515332835072)&gt;] Progress-&gt;  700|1755 T- 18: 7: 11
Thread [&lt;DummyProcess(Thread-4, started daemon 140515358013184)&gt;] Progress-&gt;  500|1756 T- 18: 8: 43
Thread [&lt;DummyProcess(Thread-6, started daemon 140515341227776)&gt;] Progress-&gt;  600|1756 T- 18: 9: 2
Thread [&lt;DummyProcess(Thread-7, started daemon 140515332835072)&gt;] Progress-&gt;  800|1755 T- 18: 9: 11
Thread [&lt;DummyProcess(Thread-5, started daemon 140515349620480)&gt;] Progress-&gt;  500|1756 T- 18: 9: 59
Thread [&lt;DummyProcess(Thread-7, started daemon 140515332835072)&gt;] Progress-&gt;  900|1755 T- 18: 11: 13
Thread [&lt;DummyProcess(Thread-6, started daemon 140515341227776)&gt;] Progress-&gt;  700|1756 T- 18: 11: 43
Thread [&lt;DummyProcess(Thread-4, started daemon 140515358013184)&gt;] Progress-&gt;  600|1756 T- 18: 11: 53
Thread [&lt;DummyProcess(Thread-7, started daemon 140515332835072)&gt;] Progress-&gt;  1000|1755 T- 18: 13: 14
Thread [&lt;DummyProcess(Thread-5, started daemon 140515349620480)&gt;] Progress-&gt;  600|1756 T- 18: 13: 32
Thread [&lt;DummyProcess(Thread-6, started daemon 140515341227776)&gt;] Progress-&gt;  800|1756 T- 18: 14: 27
Thread [&lt;DummyProcess(Thread-4, started daemon 140515358013184)&gt;] Progress-&gt;  700|1756 T- 18: 14: 52
Thread [&lt;DummyProcess(Thread-7, started daemon 140515332835072)&gt;] Progress-&gt;  1100|1755 T- 18: 15: 6
Thread [&lt;DummyProcess(Thread-5, started daemon 140515349620480)&gt;] Progress-&gt;  700|1756 T- 18: 16: 45
Thread [&lt;DummyProcess(Thread-7, started daemon 140515332835072)&gt;] Progress-&gt;  1200|1755 T- 18: 17: 1
Thread [&lt;DummyProcess(Thread-6, started daemon 140515341227776)&gt;] Progress-&gt;  900|1756 T- 18: 17: 7
Thread [&lt;DummyProcess(Thread-4, started daemon 140515358013184)&gt;] Progress-&gt;  800|1756 T- 18: 17: 58
Thread [&lt;DummyProcess(Thread-7, started daemon 140515332835072)&gt;] Progress-&gt;  1300|1755 T- 18: 18: 56
Thread [&lt;DummyProcess(Thread-5, started daemon 140515349620480)&gt;] Progress-&gt;  800|1756 T- 18: 19: 40
Thread [&lt;DummyProcess(Thread-6, started daemon 140515341227776)&gt;] Progress-&gt;  1000|1756 T- 18: 19: 51
Thread [&lt;DummyProcess(Thread-7, started daemon 140515332835072)&gt;] Progress-&gt;  1400|1755 T- 18: 20: 43
Thread [&lt;DummyProcess(Thread-4, started daemon 140515358013184)&gt;] Progress-&gt;  900|1756 T- 18: 21: 4
Thread [&lt;DummyProcess(Thread-5, started daemon 140515349620480)&gt;] Progress-&gt;  900|1756 T- 18: 21: 36
Thread [&lt;DummyProcess(Thread-7, started daemon 140515332835072)&gt;] Progress-&gt;  1500|1755 T- 18: 22: 28
Thread [&lt;DummyProcess(Thread-6, started daemon 140515341227776)&gt;] Progress-&gt;  1100|1756 T- 18: 22: 33
Thread [&lt;DummyProcess(Thread-5, started daemon 140515349620480)&gt;] Progress-&gt;  1000|1756 T- 18: 23: 30
Thread [&lt;DummyProcess(Thread-4, started daemon 140515358013184)&gt;] Progress-&gt;  1000|1756 T- 18: 24: 0
Thread [&lt;DummyProcess(Thread-7, started daemon 140515332835072)&gt;] Progress-&gt;  1600|1755 T- 18: 24: 15
Thread [&lt;DummyProcess(Thread-6, started daemon 140515341227776)&gt;] Progress-&gt;  1200|1756 T- 18: 25: 11
Thread [&lt;DummyProcess(Thread-5, started daemon 140515349620480)&gt;] Progress-&gt;  1100|1756 T- 18: 25: 24
Thread [&lt;DummyProcess(Thread-7, started daemon 140515332835072)&gt;] Progress-&gt;  1700|1755 T- 18: 26: 2
Progress-&gt;  1755|1755 T- 18: 27: 0
Thread [&lt;DummyProcess(Thread-4, started daemon 140515358013184)&gt;] Progress-&gt;  1100|1756 T- 18: 27: 5
Thread [&lt;DummyProcess(Thread-5, started daemon 140515349620480)&gt;] Progress-&gt;  1200|1756 T- 18: 27: 10
Thread [&lt;DummyProcess(Thread-6, started daemon 140515341227776)&gt;] Progress-&gt;  1300|1756 T- 18: 27: 40
Thread [&lt;DummyProcess(Thread-5, started daemon 140515349620480)&gt;] Progress-&gt;  1300|1756 T- 18: 28: 35
Thread [&lt;DummyProcess(Thread-4, started daemon 140515358013184)&gt;] Progress-&gt;  1200|1756 T- 18: 29: 24
Thread [&lt;DummyProcess(Thread-6, started daemon 140515341227776)&gt;] Progress-&gt;  1400|1756 T- 18: 29: 41
Thread [&lt;DummyProcess(Thread-5, started daemon 140515349620480)&gt;] Progress-&gt;  1400|1756 T- 18: 30: 9
Thread [&lt;DummyProcess(Thread-5, started daemon 140515349620480)&gt;] Progress-&gt;  1500|1756 T- 18: 31: 41
Thread [&lt;DummyProcess(Thread-6, started daemon 140515341227776)&gt;] Progress-&gt;  1500|1756 T- 18: 31: 44
Thread [&lt;DummyProcess(Thread-4, started daemon 140515358013184)&gt;] Progress-&gt;  1300|1756 T- 18: 31: 48
Thread [&lt;DummyProcess(Thread-5, started daemon 140515349620480)&gt;] Progress-&gt;  1600|1756 T- 18: 33: 13
Thread [&lt;DummyProcess(Thread-6, started daemon 140515341227776)&gt;] Progress-&gt;  1600|1756 T- 18: 33: 40
Thread [&lt;DummyProcess(Thread-4, started daemon 140515358013184)&gt;] Progress-&gt;  1400|1756 T- 18: 34: 6
Thread [&lt;DummyProcess(Thread-5, started daemon 140515349620480)&gt;] Progress-&gt;  1700|1756 T- 18: 34: 46
Thread [&lt;DummyProcess(Thread-6, started daemon 140515341227776)&gt;] Progress-&gt;  1700|1756 T- 18: 35: 41
Progress-&gt;  1756|1756 T- 18: 35: 45
</pre>
</div>
</div>

<div class="output_area">
<div class="prompt"></div>

<div class="output_subarea output_stream output_stderr output_text">
<pre>[Parallel(n_jobs=4)]: Done   2 out of   4 | elapsed: 42.8min remaining: 42.8min
</pre>
</div>
</div>

<div class="output_area">
<div class="prompt"></div>

<div class="output_subarea output_stream output_stdout output_text">
<pre>Thread [&lt;DummyProcess(Thread-4, started daemon 140515358013184)&gt;] Progress-&gt;  1500|1756 T- 18: 36: 14
Progress-&gt;  1756|1756 T- 18: 36: 28
Thread [&lt;DummyProcess(Thread-4, started daemon 140515358013184)&gt;] Progress-&gt;  1600|1756 T- 18: 37: 16
Thread [&lt;DummyProcess(Thread-4, started daemon 140515358013184)&gt;] Progress-&gt;  1700|1756 T- 18: 38: 11
Progress-&gt;  1756|1756 T- 18: 38: 42
Progress-&gt;  7023|7023 T- 18: 38: 42
CPU times: user 2h 15min 37s, sys: 14min 37s, total: 2h 30min 15s
Wall time: 45min 42s
</pre>
</div>
</div>

<div class="output_area">
<div class="prompt"></div>

<div class="output_subarea output_stream output_stderr output_text">
<pre>[Parallel(n_jobs=4)]: Done   4 out of   4 | elapsed: 45.7min finished
</pre>
</div>
</div>

</div>
</div>

</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[25]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="k">def</span> <span class="nf">read_data</span><span class="p">(</span><span class="n">path_to_file</span><span class="p">):</span>
    <span class="n">fs</span> <span class="o">=</span> <span class="n">cv2</span><span class="o">.</span><span class="n">FileStorage</span><span class="p">(</span><span class="n">path_to_file</span><span class="p">,</span> <span class="n">cv2</span><span class="o">.</span><span class="n">FileStorage_READ</span><span class="p">)</span>
    
    <span class="n">histograms</span> <span class="o">=</span> <span class="n">fs</span><span class="o">.</span><span class="n">getNode</span><span class="p">(</span><span class="s2">&quot;HIST&quot;</span><span class="p">)</span><span class="o">.</span><span class="n">mat</span><span class="p">()</span>
    <span class="n">labels</span> <span class="o">=</span> <span class="n">fs</span><span class="o">.</span><span class="n">getNode</span><span class="p">(</span><span class="s2">&quot;LABEL&quot;</span><span class="p">)</span><span class="o">.</span><span class="n">mat</span><span class="p">()</span>
    
    <span class="nb">print</span><span class="p">(</span><span class="s2">&quot;Data read from file:&quot;</span><span class="p">,</span><span class="n">path_to_file</span><span class="p">)</span>
    <span class="n">fs</span><span class="o">.</span><span class="n">release</span><span class="p">()</span>
    <span class="k">return</span> <span class="n">histograms</span><span class="p">,</span> <span class="n">labels</span>
<span class="c1">###############################################################</span>
<span class="k">def</span> <span class="nf">write_data</span><span class="p">(</span><span class="n">path_to_file</span><span class="p">,</span> <span class="n">histograms</span><span class="p">,</span> <span class="n">labels</span><span class="p">):</span>
    <span class="n">fs</span> <span class="o">=</span> <span class="n">cv2</span><span class="o">.</span><span class="n">FileStorage</span><span class="p">(</span><span class="n">filepath</span><span class="p">,</span> <span class="n">cv2</span><span class="o">.</span><span class="n">FileStorage_WRITE</span><span class="p">)</span>
    
    <span class="n">fs</span><span class="o">.</span><span class="n">write</span><span class="p">(</span><span class="s2">&quot;HIST&quot;</span><span class="p">,</span><span class="n">histograms</span><span class="p">)</span>
    <span class="n">fs</span><span class="o">.</span><span class="n">write</span><span class="p">(</span><span class="s2">&quot;LABEL&quot;</span><span class="p">,</span> <span class="n">labels</span><span class="p">)</span>
    
    <span class="nb">print</span><span class="p">(</span><span class="s2">&quot;Data saved to file:&quot;</span><span class="p">,</span><span class="n">path_to_file</span><span class="p">)</span>
    <span class="k">return</span> <span class="n">fs</span><span class="o">.</span><span class="n">release</span><span class="p">()</span>
</pre></div>

</div>
</div>
</div>

</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[&nbsp;]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span> 
</pre></div>

</div>
</div>
</div>

</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[26]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="k">if</span><span class="p">(</span><span class="n">detector_choice</span> <span class="o">==</span> <span class="mi">0</span><span class="p">):</span>
    <span class="n">filepath</span> <span class="o">=</span> <span class="s2">&quot;KAZE_histograms_alldb.yaml&quot;</span>
<span class="k">elif</span><span class="p">(</span><span class="n">detector_choice</span> <span class="o">==</span> <span class="mi">1</span><span class="p">):</span>
    <span class="n">filepath</span> <span class="o">=</span> <span class="s2">&quot;SURF_histograms_alldb.yaml&quot;</span>

<span class="n">write_data</span><span class="p">(</span><span class="n">filepath</span><span class="p">,</span> <span class="n">data_histograms</span><span class="p">,</span> <span class="n">data_labels</span><span class="p">)</span>
</pre></div>

</div>
</div>
</div>

<div class="output_wrapper">
<div class="output">


<div class="output_area">
<div class="prompt"></div>

<div class="output_subarea output_stream output_stdout output_text">
<pre>Data saved to file: KAZE_histograms_alldb.yaml
</pre>
</div>
</div>

</div>
</div>

</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[27]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="n">data_histograms</span><span class="p">,</span><span class="n">data_labels</span> <span class="o">=</span> <span class="n">read_data</span><span class="p">(</span><span class="n">filepath</span><span class="p">)</span>
</pre></div>

</div>
</div>
</div>

<div class="output_wrapper">
<div class="output">


<div class="output_area">
<div class="prompt"></div>

<div class="output_subarea output_stream output_stdout output_text">
<pre>Data read from file: KAZE_histograms_alldb.yaml
</pre>
</div>
</div>

</div>
</div>

</div>
<div class="cell border-box-sizing text_cell rendered">
<div class="prompt input_prompt">
</div>
<div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<h1 id="Separate-data-into-training-and-test">Separate data into training and test<a class="anchor-link" href="#Separate-data-into-training-and-test">&#182;</a></h1>
</div>
</div>
</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[28]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="k">def</span> <span class="nf">get_training_mask</span><span class="p">(</span><span class="n">size</span><span class="p">,</span> <span class="n">split_ratio</span><span class="p">):</span>
    <span class="c1"># Get a list of indices without replacement</span>
    <span class="n">indices</span> <span class="o">=</span> <span class="n">np</span><span class="o">.</span><span class="n">random</span><span class="o">.</span><span class="n">choice</span><span class="p">(</span><span class="n">size</span><span class="p">,</span> <span class="nb">int</span><span class="p">(</span><span class="n">size</span><span class="o">*</span><span class="n">split_ratio</span><span class="p">),</span><span class="n">replace</span><span class="o">=</span><span class="kc">False</span><span class="p">)</span>
    <span class="c1"># Create a False mask</span>
    <span class="n">mask</span> <span class="o">=</span> <span class="n">np</span><span class="o">.</span><span class="n">zeros</span><span class="p">((</span><span class="n">size</span><span class="p">),</span><span class="n">dtype</span><span class="o">=</span><span class="s1">&#39;bool&#39;</span><span class="p">)</span>
    <span class="c1"># Set the randomly generated indices to True</span>
    <span class="n">mask</span><span class="p">[</span><span class="n">indices</span><span class="p">]</span> <span class="o">=</span> <span class="kc">True</span>
    <span class="c1"># Return the mask</span>
    <span class="k">return</span> <span class="n">mask</span>
</pre></div>

</div>
</div>
</div>

</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[29]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="c1"># Take a portion of the samples as training</span>
<span class="n">split_ratio</span> <span class="o">=</span> <span class="mf">0.9</span>
<span class="n">training_mask</span> <span class="o">=</span> <span class="n">get_training_mask</span><span class="p">(</span><span class="nb">len</span><span class="p">(</span><span class="n">df</span><span class="p">),</span> <span class="n">split_ratio</span><span class="p">)</span>
</pre></div>

</div>
</div>
</div>

</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[30]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="c1"># Take a portion of the data as training...</span>
<span class="n">training_data</span> <span class="o">=</span> <span class="n">data_histograms</span><span class="p">[</span><span class="n">training_mask</span><span class="p">]</span>
<span class="n">training_labels</span> <span class="o">=</span> <span class="n">np</span><span class="o">.</span><span class="n">int32</span><span class="p">((</span><span class="n">df</span><span class="p">[</span><span class="n">training_mask</span><span class="p">])[</span><span class="s2">&quot;label&quot;</span><span class="p">]</span><span class="o">.</span><span class="n">values</span><span class="p">)</span>

<span class="c1"># Take the rest as test</span>
<span class="n">test_mask</span> <span class="o">=</span> <span class="o">~</span><span class="n">training_mask</span>
<span class="n">test_data</span> <span class="o">=</span> <span class="n">data_histograms</span><span class="p">[</span><span class="n">test_mask</span><span class="p">]</span>
<span class="n">test_set</span> <span class="o">=</span> <span class="n">df</span><span class="p">[</span><span class="n">test_mask</span><span class="p">]</span>
<span class="n">test_labels</span> <span class="o">=</span> <span class="n">np</span><span class="o">.</span><span class="n">int32</span><span class="p">((</span><span class="n">df</span><span class="p">[</span><span class="n">test_mask</span><span class="p">])[</span><span class="s2">&quot;label&quot;</span><span class="p">]</span><span class="o">.</span><span class="n">values</span><span class="p">)</span>
</pre></div>

</div>
</div>
</div>

</div>
<div class="cell border-box-sizing text_cell rendered">
<div class="prompt input_prompt">
</div>
<div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<h1 id="Data-prediction-using-different-models">Data prediction using different models<a class="anchor-link" href="#Data-prediction-using-different-models">&#182;</a></h1>
</div>
</div>
</div>
<div class="cell border-box-sizing text_cell rendered">
<div class="prompt input_prompt">
</div>
<div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<h1 id="Random-Forest">Random Forest<a class="anchor-link" href="#Random-Forest">&#182;</a></h1>
</div>
</div>
</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[31]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="kn">from</span> <span class="nn">sklearn</span> <span class="k">import</span> <span class="n">ensemble</span>

<span class="c1"># Define a model to predict with</span>
<span class="n">rf</span> <span class="o">=</span> <span class="n">ensemble</span><span class="o">.</span><span class="n">RandomForestClassifier</span><span class="p">(</span><span class="n">n_estimators</span><span class="o">=</span><span class="mi">32</span><span class="p">,</span><span class="n">n_jobs</span><span class="o">=</span><span class="mi">4</span><span class="p">,)</span>
<span class="n">predictor</span> <span class="o">=</span> <span class="n">rf</span>

<span class="c1"># Train a model with the training data</span>
<span class="o">%</span><span class="k">time</span> predictor.fit(training_data, training_labels)

<span class="c1"># Predict the test data&#39;s labels</span>
<span class="n">predicted_labels</span> <span class="o">=</span> <span class="n">predictor</span><span class="o">.</span><span class="n">predict</span><span class="p">(</span><span class="n">test_data</span><span class="p">)</span>
<span class="c1">#%time forest_probabilities = predictor.predict_proba(test_data)</span>

<span class="o">%</span><span class="k">time</span> print(&quot;Test:&quot;,predictor.score(test_data,test_labels))
</pre></div>

</div>
</div>
</div>

<div class="output_wrapper">
<div class="output">


<div class="output_area">
<div class="prompt"></div>

<div class="output_subarea output_stream output_stdout output_text">
<pre>CPU times: user 4.85 s, sys: 52 ms, total: 4.9 s
Wall time: 1.53 s
Test: 0.46372688478
CPU times: user 24 ms, sys: 0 ns, total: 24 ms
Wall time: 105 ms
</pre>
</div>
</div>

</div>
</div>

</div>
<div class="cell border-box-sizing text_cell rendered">
<div class="prompt input_prompt">
</div>
<div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<h1 id="Multi-layer-Perceptron">Multi-layer Perceptron<a class="anchor-link" href="#Multi-layer-Perceptron">&#182;</a></h1>
</div>
</div>
</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[32]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="kn">from</span> <span class="nn">sklearn.neural_network</span> <span class="k">import</span> <span class="n">MLPClassifier</span>

<span class="c1"># Define a model to predict with</span>
<span class="n">mlpc</span> <span class="o">=</span> <span class="n">MLPClassifier</span><span class="p">(</span><span class="n">max_iter</span><span class="o">=</span><span class="mi">400</span><span class="p">,)</span>
<span class="n">predictor</span> <span class="o">=</span> <span class="n">mlpc</span>

<span class="c1"># Train a model with the training data</span>
<span class="o">%</span><span class="k">time</span> predictor.fit(training_data, training_labels)

<span class="c1"># Predict the test data&#39;s labels</span>
<span class="n">predicted_labels</span> <span class="o">=</span> <span class="n">predictor</span><span class="o">.</span><span class="n">predict</span><span class="p">(</span><span class="n">test_data</span><span class="p">)</span>

<span class="o">%</span><span class="k">time</span> print(&quot;Test:&quot;,predictor.score(training_data,training_labels))
</pre></div>

</div>
</div>
</div>

<div class="output_wrapper">
<div class="output">


<div class="output_area">
<div class="prompt"></div>

<div class="output_subarea output_stream output_stdout output_text">
<pre>CPU times: user 2min 8s, sys: 2min 38s, total: 4min 47s
Wall time: 1min 22s
Test: 0.794462025316
CPU times: user 112 ms, sys: 68 ms, total: 180 ms
Wall time: 47.7 ms
</pre>
</div>
</div>

<div class="output_area">
<div class="prompt"></div>

<div class="output_subarea output_stream output_stderr output_text">
<pre>/usr/local/lib/python3.5/dist-packages/sklearn/neural_network/multilayer_perceptron.py:563: ConvergenceWarning: Stochastic Optimizer: Maximum iterations reached and the optimization hasn&#39;t converged yet.
  % (), ConvergenceWarning)
</pre>
</div>
</div>

</div>
</div>

</div>
<div class="cell border-box-sizing text_cell rendered">
<div class="prompt input_prompt">
</div>
<div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<h1 id="Cross-Validation">Cross Validation<a class="anchor-link" href="#Cross-Validation">&#182;</a></h1>
</div>
</div>
</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[33]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="kn">from</span> <span class="nn">sklearn.model_selection</span> <span class="k">import</span> <span class="n">cross_val_score</span>
<span class="c1">#Prepare the data</span>
<span class="n">X</span> <span class="o">=</span> <span class="n">data_histograms</span>
<span class="n">y</span> <span class="o">=</span> <span class="n">data_labels</span><span class="o">.</span><span class="n">flatten</span><span class="p">()</span>

<span class="c1"># Set K partitions</span>
<span class="n">k_fold</span> <span class="o">=</span> <span class="mi">10</span>
<span class="c1"># K-fold cross validation scoring (test errors)</span>
<span class="o">%</span><span class="k">time</span> cv_rf = cross_val_score(estimator=rf,X=X,y=y,cv=k_fold,n_jobs=joblib.cpu_count())
<span class="o">%</span><span class="k">time</span> cv_mlpc = cross_val_score(estimator=mlpc,X=X,y=y,cv=k_fold,n_jobs=joblib.cpu_count())
</pre></div>

</div>
</div>
</div>

<div class="output_wrapper">
<div class="output">


<div class="output_area">
<div class="prompt"></div>

<div class="output_subarea output_stream output_stdout output_text">
<pre>CPU times: user 540 ms, sys: 220 ms, total: 760 ms
Wall time: 14 s
</pre>
</div>
</div>

<div class="output_area">
<div class="prompt"></div>

<div class="output_subarea output_stream output_stderr output_text">
<pre>/usr/local/lib/python3.5/dist-packages/sklearn/neural_network/multilayer_perceptron.py:563: ConvergenceWarning: Stochastic Optimizer: Maximum iterations reached and the optimization hasn&#39;t converged yet.
  % (), ConvergenceWarning)
/usr/local/lib/python3.5/dist-packages/sklearn/neural_network/multilayer_perceptron.py:563: ConvergenceWarning: Stochastic Optimizer: Maximum iterations reached and the optimization hasn&#39;t converged yet.
  % (), ConvergenceWarning)
/usr/local/lib/python3.5/dist-packages/sklearn/neural_network/multilayer_perceptron.py:563: ConvergenceWarning: Stochastic Optimizer: Maximum iterations reached and the optimization hasn&#39;t converged yet.
  % (), ConvergenceWarning)
/usr/local/lib/python3.5/dist-packages/sklearn/neural_network/multilayer_perceptron.py:563: ConvergenceWarning: Stochastic Optimizer: Maximum iterations reached and the optimization hasn&#39;t converged yet.
  % (), ConvergenceWarning)
</pre>
</div>
</div>

<div class="output_area">
<div class="prompt"></div>

<div class="output_subarea output_stream output_stdout output_text">
<pre>CPU times: user 1.03 s, sys: 276 ms, total: 1.31 s
Wall time: 15min 48s
</pre>
</div>
</div>

</div>
</div>

</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[74]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="c1"># CV for the RF model</span>
<span class="nb">print</span><span class="p">(</span><span class="s2">&quot;CV Random Forest test errors:</span><span class="se">\n</span><span class="s2">&quot;</span><span class="p">,</span><span class="n">cv_rf</span><span class="p">)</span>
<span class="nb">print</span><span class="p">(</span><span class="s2">&quot;Mean test accuracy: </span><span class="si">%0.2f</span><span class="s2"> (+/- </span><span class="si">%0.2f</span><span class="s2">)&quot;</span> <span class="o">%</span> <span class="p">(</span><span class="n">cv_rf</span><span class="o">.</span><span class="n">mean</span><span class="p">(),</span> <span class="n">cv_rf</span><span class="o">.</span><span class="n">std</span><span class="p">()</span> <span class="o">*</span> <span class="mi">2</span><span class="p">))</span>
</pre></div>

</div>
</div>
</div>

<div class="output_wrapper">
<div class="output">


<div class="output_area">
<div class="prompt"></div>

<div class="output_subarea output_stream output_stdout output_text">
<pre>CV Random Forest test errors:
 [ 0.2132964   0.22517483  0.18487395  0.19350282  0.23044097  0.25178827
  0.29640288  0.2301013   0.24092888  0.23726346]
Mean test accuracy: 0.23 (+/- 0.06)
</pre>
</div>
</div>

</div>
</div>

</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[35]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="c1"># CV for the MLPC model</span>
<span class="nb">print</span><span class="p">(</span><span class="s2">&quot;CV Multi-Layer Perceptron Classifier test errors:</span><span class="se">\n</span><span class="s2">&quot;</span><span class="p">,</span><span class="n">cv_rf</span><span class="p">)</span>
<span class="nb">print</span><span class="p">(</span><span class="s2">&quot;Mean test accuracy: </span><span class="si">%0.2f</span><span class="s2"> (+/- </span><span class="si">%0.2f</span><span class="s2">)&quot;</span> <span class="o">%</span> <span class="p">(</span><span class="n">cv_mlpc</span><span class="o">.</span><span class="n">mean</span><span class="p">(),</span> <span class="n">cv_mlpc</span><span class="o">.</span><span class="n">std</span><span class="p">()</span> <span class="o">*</span> <span class="mi">2</span><span class="p">))</span>
</pre></div>

</div>
</div>
</div>

<div class="output_wrapper">
<div class="output">


<div class="output_area">
<div class="prompt"></div>

<div class="output_subarea output_stream output_stdout output_text">
<pre>CV Multi-Layer Perceptron Classifier test errors:
 [ 0.2132964   0.22517483  0.18487395  0.19350282  0.23044097  0.25178827
  0.29640288  0.2301013   0.24092888  0.23726346]
Mean test accuracy: 0.29 (+/- 0.10)
</pre>
</div>
</div>

</div>
</div>

</div>
<div class="cell border-box-sizing text_cell rendered">
<div class="prompt input_prompt">
</div>
<div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<h1 id="SVM-(OpenCV)">SVM (OpenCV)<a class="anchor-link" href="#SVM-(OpenCV)">&#182;</a></h1>
</div>
</div>
</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[36]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="c1"># Define a model to predict with</span>
<span class="n">predictor</span> <span class="o">=</span> <span class="n">cv2</span><span class="o">.</span><span class="n">ml</span><span class="o">.</span><span class="n">SVM_create</span><span class="p">();</span>

<span class="n">predictor</span><span class="o">.</span><span class="n">setKernel</span><span class="p">(</span><span class="n">cv2</span><span class="o">.</span><span class="n">ml</span><span class="o">.</span><span class="n">SVM_RBF</span><span class="p">)</span>
<span class="n">predictor</span><span class="o">.</span><span class="n">setType</span><span class="p">(</span><span class="n">cv2</span><span class="o">.</span><span class="n">ml</span><span class="o">.</span><span class="n">SVM_C_SVC</span><span class="p">)</span>
<span class="n">predictor</span><span class="o">.</span><span class="n">setC</span><span class="p">(</span><span class="mf">62.5</span><span class="p">)</span>
<span class="n">predictor</span><span class="o">.</span><span class="n">setGamma</span><span class="p">(</span><span class="mf">0.50625</span><span class="p">)</span>

<span class="c1"># Train a model with the training data</span>
<span class="o">%</span><span class="k">time</span> predictor.train(training_data, cv2.ml.ROW_SAMPLE, training_labels)

<span class="c1"># Predict the test data&#39;s labels</span>
<span class="n">retval</span><span class="p">,</span> <span class="n">predicted_labels</span> <span class="o">=</span> <span class="n">predictor</span><span class="o">.</span><span class="n">predict</span><span class="p">(</span><span class="n">test_data</span><span class="p">)</span>
</pre></div>

</div>
</div>
</div>

<div class="output_wrapper">
<div class="output">


<div class="output_area">
<div class="prompt"></div>

<div class="output_subarea output_stream output_stdout output_text">
<pre>CPU times: user 40.1 s, sys: 48 ms, total: 40.2 s
Wall time: 40.3 s
</pre>
</div>
</div>

</div>
</div>

</div>
<div class="cell border-box-sizing text_cell rendered">
<div class="prompt input_prompt">
</div>
<div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<h1 id="Compute-the-confusion-matrix">Compute the confusion matrix<a class="anchor-link" href="#Compute-the-confusion-matrix">&#182;</a></h1>
</div>
</div>
</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[37]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="k">def</span> <span class="nf">compute_confusion_matrix</span><span class="p">(</span><span class="n">actual</span><span class="p">,</span> <span class="n">predicted</span><span class="p">):</span>
    <span class="n">N</span> <span class="o">=</span> <span class="mi">47</span>
    <span class="n">conf_matrix</span> <span class="o">=</span> <span class="n">np</span><span class="o">.</span><span class="n">zeros</span><span class="p">((</span><span class="n">N</span><span class="p">,</span><span class="n">N</span><span class="p">),</span> <span class="n">dtype</span><span class="o">=</span><span class="s1">&#39;int32&#39;</span><span class="p">)</span>
    
    <span class="k">for</span> <span class="n">ii</span> <span class="ow">in</span> <span class="nb">range</span><span class="p">(</span><span class="nb">len</span><span class="p">(</span><span class="n">predicted_labels</span><span class="p">)):</span>
        <span class="n">conf_matrix</span><span class="p">[</span><span class="n">test_labels</span><span class="p">[</span><span class="n">ii</span><span class="p">]][</span><span class="n">predicted_labels</span><span class="p">[</span><span class="n">ii</span><span class="p">]]</span><span class="o">+=</span><span class="mi">1</span>
    
    <span class="k">return</span> <span class="n">conf_matrix</span>
</pre></div>

</div>
</div>
</div>

</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[75]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="c1"># Random Forest: we compute its confusion matrix and evaluate its results</span>

<span class="c1"># Flatten the Nsamples,1 matrix into a Nsamples array</span>
<span class="n">predicted_labels</span> <span class="o">=</span> <span class="n">np</span><span class="o">.</span><span class="n">int32</span><span class="p">(</span><span class="n">predicted_labels</span><span class="o">.</span><span class="n">flatten</span><span class="p">())</span>

<span class="c1"># compute the confusion matrix</span>
<span class="n">conf_matrix</span> <span class="o">=</span> <span class="n">compute_confusion_matrix</span><span class="p">(</span><span class="n">test_labels</span><span class="p">,</span> <span class="n">predicted_labels</span><span class="p">)</span>
</pre></div>

</div>
</div>
</div>

</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[39]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="nb">print</span><span class="p">(</span><span class="s2">&quot;Test right predictions-&gt;&quot;</span><span class="p">,</span> <span class="n">conf_matrix</span><span class="o">.</span><span class="n">trace</span><span class="p">(),</span><span class="s2">&quot;|&quot;</span><span class="p">,</span><span class="nb">str</span><span class="p">(</span><span class="n">conf_matrix</span><span class="o">.</span><span class="n">trace</span><span class="p">()</span><span class="o">/</span><span class="n">np</span><span class="o">.</span><span class="n">sum</span><span class="p">(</span><span class="n">conf_matrix</span><span class="p">,(</span><span class="mi">0</span><span class="p">,</span><span class="mi">1</span><span class="p">))</span><span class="o">*</span><span class="mi">100</span><span class="p">)</span><span class="o">+</span><span class="s2">&quot;%&quot;</span><span class="p">)</span>
<span class="nb">print</span><span class="p">(</span><span class="s2">&quot;Test wrong predictions-&gt;&quot;</span><span class="p">,</span> <span class="n">np</span><span class="o">.</span><span class="n">sum</span><span class="p">(</span><span class="n">conf_matrix</span><span class="p">,(</span><span class="mi">0</span><span class="p">,</span><span class="mi">1</span><span class="p">))</span><span class="o">-</span><span class="n">conf_matrix</span><span class="o">.</span><span class="n">trace</span><span class="p">(),</span><span class="s2">&quot;|&quot;</span><span class="p">,</span><span class="nb">str</span><span class="p">((</span><span class="mi">1</span><span class="o">-</span><span class="n">conf_matrix</span><span class="o">.</span><span class="n">trace</span><span class="p">()</span><span class="o">/</span><span class="n">np</span><span class="o">.</span><span class="n">sum</span><span class="p">(</span><span class="n">conf_matrix</span><span class="p">,(</span><span class="mi">0</span><span class="p">,</span><span class="mi">1</span><span class="p">)))</span><span class="o">*</span><span class="mi">100</span><span class="p">)</span><span class="o">+</span><span class="s2">&quot;%&quot;</span><span class="p">)</span>
</pre></div>

</div>
</div>
</div>

<div class="output_wrapper">
<div class="output">


<div class="output_area">
<div class="prompt"></div>

<div class="output_subarea output_stream output_stdout output_text">
<pre>Test right predictions-&gt; 276 | 39.2603129445%
Test wrong predictions-&gt; 427 | 60.7396870555%
</pre>
</div>
</div>

</div>
</div>

</div>
<div class="cell border-box-sizing text_cell rendered">
<div class="prompt input_prompt">
</div>
<div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<h1 id="Random-Forest's-confusion-matrix">Random Forest's confusion matrix<a class="anchor-link" href="#Random-Forest's-confusion-matrix">&#182;</a></h1>
</div>
</div>
</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[76]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="c1"># Define a model to predict with</span>
<span class="n">rf</span> <span class="o">=</span> <span class="n">ensemble</span><span class="o">.</span><span class="n">RandomForestClassifier</span><span class="p">(</span><span class="n">n_estimators</span><span class="o">=</span><span class="mi">32</span><span class="p">,</span><span class="n">n_jobs</span><span class="o">=</span><span class="mi">4</span><span class="p">,)</span>
<span class="n">predictor</span> <span class="o">=</span> <span class="n">rf</span>

<span class="c1"># Train a model with the training data</span>
<span class="n">predictor</span><span class="o">.</span><span class="n">fit</span><span class="p">(</span><span class="n">training_data</span><span class="p">,</span> <span class="n">training_labels</span><span class="p">)</span>

<span class="c1"># Predict the test data&#39;s labels</span>
<span class="n">predicted_labels</span> <span class="o">=</span> <span class="n">predictor</span><span class="o">.</span><span class="n">predict</span><span class="p">(</span><span class="n">test_data</span><span class="p">)</span>
</pre></div>

</div>
</div>
</div>

</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[77]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="c1"># Flatten the Nsamples,1 matrix into a Nsamples array</span>
<span class="n">predicted_labels</span> <span class="o">=</span> <span class="n">np</span><span class="o">.</span><span class="n">int32</span><span class="p">(</span><span class="n">predicted_labels</span><span class="o">.</span><span class="n">flatten</span><span class="p">())</span>

<span class="c1"># compute the confusion matrix</span>
<span class="n">conf_matrix</span> <span class="o">=</span> <span class="n">compute_confusion_matrix</span><span class="p">(</span><span class="n">test_labels</span><span class="p">,</span> <span class="n">predicted_labels</span><span class="p">)</span>
</pre></div>

</div>
</div>
</div>

</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[78]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="c1"># Evaluate the results</span>
<span class="nb">print</span><span class="p">(</span><span class="s2">&quot;Total samples-&gt;&quot;</span><span class="p">,</span><span class="nb">len</span><span class="p">(</span><span class="n">data_frame</span><span class="p">))</span>
<span class="nb">print</span><span class="p">(</span><span class="s2">&quot;Used samples(more than 0 kp detected)-&gt;&quot;</span><span class="p">,</span> <span class="nb">len</span><span class="p">(</span><span class="n">df</span><span class="p">))</span>
<span class="nb">print</span><span class="p">(</span><span class="s2">&quot;Discarded samples(0 kp detected)-&gt;&quot;</span><span class="p">,</span> <span class="nb">len</span><span class="p">(</span><span class="n">data_frame</span><span class="p">)</span><span class="o">-</span><span class="nb">len</span><span class="p">(</span><span class="n">df</span><span class="p">))</span>
<span class="nb">print</span><span class="p">(</span><span class="s2">&quot;----------------------------------------------------------------------&quot;</span><span class="p">)</span>
<span class="nb">print</span><span class="p">(</span><span class="s2">&quot;BOW size-&gt;&quot;</span><span class="p">,</span> <span class="n">n_centroids</span><span class="p">,</span><span class="s2">&quot;words&quot;</span><span class="p">)</span>
<span class="nb">print</span><span class="p">(</span><span class="s2">&quot;----------------------------------------------------------------------&quot;</span><span class="p">)</span>
<span class="nb">print</span><span class="p">(</span><span class="s2">&quot;Training/Test split ratio-&gt;&quot;</span><span class="p">,</span> <span class="n">split_ratio</span><span class="p">)</span>
<span class="nb">print</span><span class="p">(</span><span class="s2">&quot;Training samples-&gt;&quot;</span><span class="p">,</span> <span class="nb">len</span><span class="p">(</span><span class="n">training_labels</span><span class="p">))</span>
<span class="nb">print</span><span class="p">(</span><span class="s2">&quot;Test samples-&gt;&quot;</span><span class="p">,</span><span class="nb">len</span><span class="p">(</span><span class="n">test_labels</span><span class="p">))</span>
<span class="nb">print</span><span class="p">(</span><span class="s2">&quot;----------------------------------------------------------------------&quot;</span><span class="p">)</span>
<span class="c1">#print(&quot;Model used-&gt;&quot;, predictor.getDefaultName())</span>
<span class="nb">print</span><span class="p">(</span><span class="s2">&quot;Test right predictions-&gt;&quot;</span><span class="p">,</span> <span class="n">conf_matrix</span><span class="o">.</span><span class="n">trace</span><span class="p">(),</span><span class="s2">&quot;|&quot;</span><span class="p">,</span><span class="nb">str</span><span class="p">(</span><span class="n">conf_matrix</span><span class="o">.</span><span class="n">trace</span><span class="p">()</span><span class="o">/</span><span class="n">np</span><span class="o">.</span><span class="n">sum</span><span class="p">(</span><span class="n">conf_matrix</span><span class="p">,(</span><span class="mi">0</span><span class="p">,</span><span class="mi">1</span><span class="p">))</span><span class="o">*</span><span class="mi">100</span><span class="p">)</span><span class="o">+</span><span class="s2">&quot;%&quot;</span><span class="p">)</span>
<span class="nb">print</span><span class="p">(</span><span class="s2">&quot;Test wrong predictions-&gt;&quot;</span><span class="p">,</span> <span class="n">np</span><span class="o">.</span><span class="n">sum</span><span class="p">(</span><span class="n">conf_matrix</span><span class="p">,(</span><span class="mi">0</span><span class="p">,</span><span class="mi">1</span><span class="p">))</span><span class="o">-</span><span class="n">conf_matrix</span><span class="o">.</span><span class="n">trace</span><span class="p">(),</span><span class="s2">&quot;|&quot;</span><span class="p">,</span><span class="nb">str</span><span class="p">((</span><span class="mi">1</span><span class="o">-</span><span class="n">conf_matrix</span><span class="o">.</span><span class="n">trace</span><span class="p">()</span><span class="o">/</span><span class="n">np</span><span class="o">.</span><span class="n">sum</span><span class="p">(</span><span class="n">conf_matrix</span><span class="p">,(</span><span class="mi">0</span><span class="p">,</span><span class="mi">1</span><span class="p">)))</span><span class="o">*</span><span class="mi">100</span><span class="p">)</span><span class="o">+</span><span class="s2">&quot;%&quot;</span><span class="p">)</span>
</pre></div>

</div>
</div>
</div>

<div class="output_wrapper">
<div class="output">


<div class="output_area">
<div class="prompt"></div>

<div class="output_subarea output_stream output_stdout output_text">
<pre>Total samples-&gt; 7855
Used samples(more than 0 kp detected)-&gt; 7023
Discarded samples(0 kp detected)-&gt; 832
----------------------------------------------------------------------
BOW size-&gt; 1024 words
----------------------------------------------------------------------
Training/Test split ratio-&gt; 0.9
Training samples-&gt; 6320
Test samples-&gt; 703
----------------------------------------------------------------------
Test right predictions-&gt; 339 | 48.2219061166%
Test wrong predictions-&gt; 364 | 51.7780938834%
</pre>
</div>
</div>

</div>
</div>

</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[79]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="nb">print</span><span class="p">(</span><span class="s2">&quot;Confusion Matrix:&quot;</span><span class="p">)</span>
<span class="n">actual</span> <span class="o">=</span> <span class="n">pd</span><span class="o">.</span><span class="n">Series</span><span class="p">(</span><span class="n">test_labels</span><span class="p">,</span> <span class="n">name</span><span class="o">=</span><span class="s2">&quot;Actual&quot;</span><span class="p">)</span>

<span class="n">predicted</span> <span class="o">=</span> <span class="n">pd</span><span class="o">.</span><span class="n">Series</span><span class="p">(</span><span class="n">predicted_labels</span><span class="p">,</span> <span class="n">name</span><span class="o">=</span><span class="s2">&quot;Real\Predicted&quot;</span><span class="p">)</span>
<span class="n">confmat</span> <span class="o">=</span> <span class="n">pd</span><span class="o">.</span><span class="n">crosstab</span><span class="p">(</span><span class="n">actual</span><span class="p">,</span> <span class="n">predicted</span><span class="p">)</span>

<span class="n">confmat</span><span class="o">.</span><span class="n">columns</span> <span class="o">=</span> <span class="n">sign_categories</span><span class="o">.</span><span class="n">categories</span><span class="p">[</span><span class="n">np</span><span class="o">.</span><span class="n">int32</span><span class="p">(</span><span class="n">confmat</span><span class="o">.</span><span class="n">columns</span><span class="p">)]</span>
<span class="n">confmat</span> <span class="o">=</span> <span class="n">confmat</span><span class="o">.</span><span class="n">set_index</span><span class="p">(</span><span class="n">sign_categories</span><span class="o">.</span><span class="n">categories</span><span class="p">[</span><span class="n">confmat</span><span class="o">.</span><span class="n">index</span><span class="p">])</span>
<span class="nb">print</span><span class="p">(</span><span class="n">confmat</span><span class="p">)</span>
</pre></div>

</div>
</div>
</div>

<div class="output_wrapper">
<div class="output">


<div class="output_area">
<div class="prompt"></div>

<div class="output_subarea output_stream output_stdout output_text">
<pre>Confusion Matrix:
                      addedLane  curveRight  doNotEnter  keepRight  laneEnds  \
addedLane                    11           0           0          0         1   
curveLeft                     1           0           0          0         0   
curveRight                    0           2           0          0         0   
dip                           0           0           0          0         0   
doNotEnter                    0           0           0          0         0   
keepRight                     0           0           0          7         0   
laneEnds                      1           0           0          0         3   
merge                         2           0           0          0         0   
noLeftTurn                    0           0           0          0         0   
noRightTurn                   0           0           0          0         0   
pedestrianCrossing            1           0           0          0         0   
rampSpeedAdvisory45           0           0           0          0         0   
rampSpeedAdvisory50           0           0           0          0         0   
rightLaneMustTurn             0           0           0          0         0   
roundabout                    0           0           0          0         0   
school                        0           0           0          0         0   
schoolSpeedLimit25            0           0           0          0         0   
signalAhead                   2           0           0          1         2   
slow                          0           0           0          0         0   
speedLimit15                  0           0           0          0         0   
speedLimit25                  0           0           0          0         0   
speedLimit30                  0           0           0          0         0   
speedLimit35                  0           0           0          0         0   
speedLimit40                  0           0           0          0         0   
speedLimit45                  0           0           0          1         0   
speedLimit50                  0           0           0          5         0   
speedLimit65                  2           0           0          0         0   
speedLimitUrdbl               0           0           0          1         0   
stop                          1           0           2          0         0   
stopAhead                     0           0           0          0         0   
thruTrafficMergeLeft          0           0           0          0         0   
truckSpeedLimit55             1           0           0          0         0   
turnLeft                      0           0           0          0         0   
turnRight                     0           0           0          0         0   
yield                         1           0           0          0         0   
yieldAhead                    1           0           0          0         0   
zoneAhead45                   0           0           0          0         0   

                      merge  noRightTurn  pedestrianCrossing  \
addedLane                 1            0                   1   
curveLeft                 0            0                   0   
curveRight                0            0                   3   
dip                       0            0                   1   
doNotEnter                0            0                   0   
keepRight                 0            0                   9   
laneEnds                  0            0                   7   
merge                     9            0                   3   
noLeftTurn                0            0                   0   
noRightTurn               0            0                   0   
pedestrianCrossing        0            1                  69   
rampSpeedAdvisory45       1            0                   0   
rampSpeedAdvisory50       0            0                   0   
rightLaneMustTurn         0            0                   0   
roundabout                0            0                   2   
school                    0            0                   1   
schoolSpeedLimit25        0            0                   2   
signalAhead               1            3                   6   
slow                      0            0                   0   
speedLimit15              0            0                   0   
speedLimit25              1            0                   6   
speedLimit30              0            0                   6   
speedLimit35              1            0                  13   
speedLimit40              0            0                   0   
speedLimit45              0            0                   2   
speedLimit50              0            0                   0   
speedLimit65              2            0                   2   
speedLimitUrdbl           1            0                   0   
stop                      0            0                  14   
stopAhead                 0            0                   4   
thruTrafficMergeLeft      1            0                   0   
truckSpeedLimit55         1            0                   0   
turnLeft                  0            0                   0   
turnRight                 0            0                   4   
yield                     2            0                  10   
yieldAhead                1            0                   1   
zoneAhead45               0            0                   0   

                      rampSpeedAdvisory45  rightLaneMustTurn     ...       \
addedLane                               0                  0     ...        
curveLeft                               0                  0     ...        
curveRight                              0                  0     ...        
dip                                     0                  0     ...        
doNotEnter                              0                  0     ...        
keepRight                               0                  0     ...        
laneEnds                                0                  0     ...        
merge                                   0                  0     ...        
noLeftTurn                              0                  0     ...        
noRightTurn                             0                  0     ...        
pedestrianCrossing                      0                  0     ...        
rampSpeedAdvisory45                     1                  0     ...        
rampSpeedAdvisory50                     0                  0     ...        
rightLaneMustTurn                       0                  1     ...        
roundabout                              0                  0     ...        
school                                  0                  0     ...        
schoolSpeedLimit25                      0                  0     ...        
signalAhead                             0                  0     ...        
slow                                    0                  0     ...        
speedLimit15                            0                  0     ...        
speedLimit25                            0                  0     ...        
speedLimit30                            0                  0     ...        
speedLimit35                            0                  0     ...        
speedLimit40                            0                  0     ...        
speedLimit45                            0                  0     ...        
speedLimit50                            0                  0     ...        
speedLimit65                            0                  0     ...        
speedLimitUrdbl                         0                  0     ...        
stop                                    0                  0     ...        
stopAhead                               0                  0     ...        
thruTrafficMergeLeft                    0                  0     ...        
truckSpeedLimit55                       0                  0     ...        
turnLeft                                0                  0     ...        
turnRight                               0                  0     ...        
yield                                   0                  0     ...        
yieldAhead                              0                  0     ...        
zoneAhead45                             0                  0     ...        

                      speedLimit45  speedLimit65  speedLimitUrdbl  stop  \
addedLane                        0             0                0     9   
curveLeft                        0             0                0     0   
curveRight                       0             0                0     1   
dip                              0             0                0     4   
doNotEnter                       0             0                0     2   
keepRight                        0             0                0    12   
laneEnds                         0             0                0     4   
merge                            0             0                0     7   
noLeftTurn                       0             0                0     1   
noRightTurn                      0             0                0     1   
pedestrianCrossing               0             0                0    21   
rampSpeedAdvisory45              0             0                0     1   
rampSpeedAdvisory50              0             0                0     1   
rightLaneMustTurn                0             0                0     3   
roundabout                       0             0                0     2   
school                           0             0                0     1   
schoolSpeedLimit25               0             0                0     1   
signalAhead                      0             0                0    21   
slow                             0             0                0     0   
speedLimit15                     0             0                0     1   
speedLimit25                     0             0                0     9   
speedLimit30                     0             0                0     3   
speedLimit35                     0             0                0    12   
speedLimit40                     0             0                0     3   
speedLimit45                     5             0                1     4   
speedLimit50                     0             0                0     0   
speedLimit65                     0             2                0     1   
speedLimitUrdbl                  0             0                2     2   
stop                             0             0                0   138   
stopAhead                        0             0                0     9   
thruTrafficMergeLeft             0             0                0     0   
truckSpeedLimit55                0             0                0     1   
turnLeft                         0             0                0     1   
turnRight                        0             0                0     2   
yield                            0             0                0     5   
yieldAhead                       0             0                0     1   
zoneAhead45                      0             0                0     0   

                      stopAhead  turnLeft  turnRight  yield  yieldAhead  \
addedLane                     0         0          0      0           0   
curveLeft                     0         0          0      0           0   
curveRight                    0         0          0      0           0   
dip                           0         0          0      0           0   
doNotEnter                    0         0          0      0           0   
keepRight                     0         0          0      1           0   
laneEnds                      0         0          0      0           0   
merge                         0         0          0      1           0   
noLeftTurn                    0         0          0      0           0   
noRightTurn                   0         0          0      0           0   
pedestrianCrossing            0         0          0      7           0   
rampSpeedAdvisory45           0         0          0      0           0   
rampSpeedAdvisory50           0         0          0      0           0   
rightLaneMustTurn             0         0          0      0           0   
roundabout                    0         0          0      0           0   
school                        0         0          0      0           0   
schoolSpeedLimit25            0         0          0      0           0   
signalAhead                   1         0          0      0           0   
slow                          0         4          1      0           0   
speedLimit15                  0         0          0      0           0   
speedLimit25                  0         0          0      0           0   
speedLimit30                  0         0          0      0           0   
speedLimit35                  0         0          0      0           0   
speedLimit40                  0         0          0      0           0   
speedLimit45                  0         0          0      0           0   
speedLimit50                  0         0          0      0           0   
speedLimit65                  0         0          0      0           0   
speedLimitUrdbl               0         0          0      0           0   
stop                          0         0          0      0           0   
stopAhead                     1         0          0      0           0   
thruTrafficMergeLeft          0         0          0      0           0   
truckSpeedLimit55             0         0          0      0           0   
turnLeft                      0         0          0      0           0   
turnRight                     0         0          1      0           0   
yield                         0         0          0      5           0   
yieldAhead                    0         0          0      0           2   
zoneAhead45                   0         0          0      0           0   

                      zoneAhead45  
addedLane                       0  
curveLeft                       0  
curveRight                      0  
dip                             0  
doNotEnter                      0  
keepRight                       0  
laneEnds                        0  
merge                           0  
noLeftTurn                      0  
noRightTurn                     0  
pedestrianCrossing              0  
rampSpeedAdvisory45             0  
rampSpeedAdvisory50             0  
rightLaneMustTurn               0  
roundabout                      0  
school                          0  
schoolSpeedLimit25              0  
signalAhead                     0  
slow                            0  
speedLimit15                    0  
speedLimit25                    0  
speedLimit30                    0  
speedLimit35                    0  
speedLimit40                    0  
speedLimit45                    0  
speedLimit50                    0  
speedLimit65                    0  
speedLimitUrdbl                 0  
stop                            0  
stopAhead                       0  
thruTrafficMergeLeft            0  
truckSpeedLimit55               0  
turnLeft                        0  
turnRight                       0  
yield                           0  
yieldAhead                      0  
zoneAhead45                     1  

[37 rows x 28 columns]
</pre>
</div>
</div>

</div>
</div>

</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[80]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="k">def</span> <span class="nf">compute_precision</span><span class="p">(</span><span class="n">c_mat</span><span class="p">):</span>
    <span class="c1"># Reduce by cols</span>
    <span class="n">totals</span> <span class="o">=</span> <span class="n">np</span><span class="o">.</span><span class="n">sum</span><span class="p">(</span><span class="n">c_mat</span><span class="p">,</span><span class="mi">0</span><span class="p">)</span>
    
    <span class="n">totals</span> <span class="o">=</span> <span class="n">np</span><span class="o">.</span><span class="n">float32</span><span class="p">(</span><span class="n">totals</span><span class="p">)</span>
    <span class="c1"># Mask rows that have no values</span>
    <span class="n">mask</span> <span class="o">=</span> <span class="n">totals</span><span class="o">&gt;</span><span class="mi">0</span>
    
    <span class="k">for</span> <span class="n">ii</span> <span class="ow">in</span> <span class="nb">range</span><span class="p">(</span><span class="nb">len</span><span class="p">(</span><span class="n">totals</span><span class="p">)):</span>
        <span class="k">if</span><span class="p">(</span><span class="n">totals</span><span class="p">[</span><span class="n">ii</span><span class="p">]</span> <span class="o">&gt;</span> <span class="mi">0</span><span class="p">):</span>
            <span class="n">totals</span><span class="p">[</span><span class="n">ii</span><span class="p">]</span> <span class="o">=</span> <span class="n">c_mat</span><span class="p">[</span><span class="n">ii</span><span class="p">,</span><span class="n">ii</span><span class="p">]</span> <span class="o">/</span> <span class="n">totals</span><span class="p">[</span><span class="n">ii</span><span class="p">]</span>
            <span class="k">pass</span>
    
    
    <span class="c1"># Drop rows that have no values (no test instances)</span>
    <span class="k">return</span> <span class="n">totals</span><span class="p">[</span><span class="n">mask</span><span class="p">]</span>
</pre></div>

</div>
</div>
</div>

</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[81]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="c1"># This means that for each time that the predicted label is one of these</span>
<span class="c1"># the predictor was right in the corresponding fraction of predictions (TP/TP+FP)</span>
<span class="n">precision</span> <span class="o">=</span> <span class="n">pd</span><span class="o">.</span><span class="n">DataFrame</span><span class="p">(</span><span class="n">compute_precision</span><span class="p">(</span><span class="n">conf_matrix</span><span class="p">),</span> <span class="n">columns</span><span class="o">=</span><span class="p">[</span><span class="s2">&quot;Prediction precision&quot;</span><span class="p">])</span>
<span class="n">precision</span> <span class="o">=</span> <span class="n">precision</span><span class="o">.</span><span class="n">set_index</span><span class="p">(</span><span class="n">confmat</span><span class="o">.</span><span class="n">columns</span><span class="p">)</span>

<span class="nb">print</span><span class="p">(</span><span class="n">precision</span><span class="p">)</span>
</pre></div>

</div>
</div>
</div>

<div class="output_wrapper">
<div class="output">


<div class="output_area">
<div class="prompt"></div>

<div class="output_subarea output_stream output_stdout output_text">
<pre>                     Prediction precision
addedLane                        0.458333
curveRight                       1.000000
doNotEnter                       0.000000
keepRight                        0.466667
laneEnds                         0.500000
merge                            0.409091
noRightTurn                      0.000000
pedestrianCrossing               0.415663
rampSpeedAdvisory45              1.000000
rightLaneMustTurn                1.000000
roundabout                       0.000000
school                           0.666667
schoolSpeedLimit25               0.666667
signalAhead                      0.550000
slow                             0.000000
speedLimit25                     0.800000
speedLimit30                     0.400000
speedLimit35                     0.823529
speedLimit45                     1.000000
speedLimit65                     1.000000
speedLimitUrdbl                  0.666667
stop                             0.485915
stopAhead                        0.500000
turnLeft                         0.000000
turnRight                        0.500000
yield                            0.357143
yieldAhead                       1.000000
zoneAhead45                      1.000000
</pre>
</div>
</div>

</div>
</div>

</div>
<div class="cell border-box-sizing text_cell rendered">
<div class="prompt input_prompt">
</div>
<div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<h1 id="Testing-using-external-test-cases">Testing using external test cases<a class="anchor-link" href="#Testing-using-external-test-cases">&#182;</a></h1>
</div>
</div>
</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[50]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="n">df_external_testing</span> <span class="o">=</span> <span class="n">pd</span><span class="o">.</span><span class="n">read_csv</span><span class="p">(</span><span class="s2">&quot;./Test_streetview/TestAnnotations.csv&quot;</span><span class="p">,</span><span class="n">sep</span><span class="o">=</span><span class="s1">&#39;;&#39;</span><span class="p">)</span>
<span class="n">df_external_testing</span>
</pre></div>

</div>
</div>
</div>

<div class="output_wrapper">
<div class="output">


<div class="output_area">
<div class="prompt output_prompt">Out[50]:</div>


<div class="output_html rendered_html output_subarea output_execute_result">
<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Filename</th>
      <th>Annotation tag</th>
      <th>label</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>./Test_streetview/noLeftTurn.jpg</td>
      <td>noLeftTurn</td>
      <td>10</td>
    </tr>
    <tr>
      <th>1</th>
      <td>./Test_streetview/pedestrianCrossing.jpg</td>
      <td>pedestrianCrossing</td>
      <td>12</td>
    </tr>
    <tr>
      <th>2</th>
      <td>./Test_streetview/pedestrianCrossing_2.jpg</td>
      <td>school</td>
      <td>21</td>
    </tr>
    <tr>
      <th>3</th>
      <td>./Test_streetview/pedestrianCrossing_3.jpg</td>
      <td>school</td>
      <td>21</td>
    </tr>
    <tr>
      <th>4</th>
      <td>./Test_streetview/pedestrianCrossing_4.jpg</td>
      <td>school</td>
      <td>21</td>
    </tr>
    <tr>
      <th>5</th>
      <td>./Test_streetview/school.jpg</td>
      <td>school</td>
      <td>21</td>
    </tr>
    <tr>
      <th>6</th>
      <td>./Test_streetview/speedLimit25.jpg</td>
      <td>speedLimit25</td>
      <td>26</td>
    </tr>
    <tr>
      <th>7</th>
      <td>./Test_streetview/stop.jpg</td>
      <td>stop</td>
      <td>35</td>
    </tr>
    <tr>
      <th>8</th>
      <td>./Test_streetview/stop_2.jpg</td>
      <td>stop</td>
      <td>35</td>
    </tr>
    <tr>
      <th>9</th>
      <td>./Test_streetview/stop_3.jpg</td>
      <td>stop</td>
      <td>35</td>
    </tr>
    <tr>
      <th>10</th>
      <td>./Test_streetview/stop_4.jpg</td>
      <td>stop</td>
      <td>35</td>
    </tr>
    <tr>
      <th>11</th>
      <td>./Test_streetview/stop_5.jpg</td>
      <td>stop</td>
      <td>35</td>
    </tr>
    <tr>
      <th>12</th>
      <td>./Test_streetview/stopAhead.jpg</td>
      <td>stopAhead</td>
      <td>36</td>
    </tr>
    <tr>
      <th>13</th>
      <td>./Test_streetview/yield.jpg</td>
      <td>yield</td>
      <td>43</td>
    </tr>
    <tr>
      <th>14</th>
      <td>./Test_streetview/ped_cross_0.jpg</td>
      <td>pedestrianCrossing</td>
      <td>12</td>
    </tr>
    <tr>
      <th>15</th>
      <td>./Test_streetview/ped_cross_1.jpg</td>
      <td>pedestrianCrossing</td>
      <td>12</td>
    </tr>
  </tbody>
</table>
</div>
</div>

</div>

</div>
</div>

</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[51]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="n">show_image</span><span class="p">(</span><span class="n">cv2</span><span class="o">.</span><span class="n">imread</span><span class="p">(</span><span class="n">df_external_testing</span><span class="o">.</span><span class="n">iloc</span><span class="p">[</span><span class="mi">0</span><span class="p">][</span><span class="s2">&quot;Filename&quot;</span><span class="p">],</span><span class="mi">0</span><span class="p">))</span>
<span class="n">show_image</span><span class="p">(</span><span class="n">cv2</span><span class="o">.</span><span class="n">imread</span><span class="p">(</span><span class="n">df_external_testing</span><span class="o">.</span><span class="n">iloc</span><span class="p">[</span><span class="mi">7</span><span class="p">][</span><span class="s2">&quot;Filename&quot;</span><span class="p">],</span><span class="mi">0</span><span class="p">))</span>
<span class="n">show_image</span><span class="p">(</span><span class="n">cv2</span><span class="o">.</span><span class="n">imread</span><span class="p">(</span><span class="n">df_external_testing</span><span class="o">.</span><span class="n">iloc</span><span class="p">[</span><span class="mi">11</span><span class="p">][</span><span class="s2">&quot;Filename&quot;</span><span class="p">],</span><span class="mi">0</span><span class="p">))</span>
<span class="n">show_image</span><span class="p">(</span><span class="n">cv2</span><span class="o">.</span><span class="n">imread</span><span class="p">(</span><span class="n">df_external_testing</span><span class="o">.</span><span class="n">iloc</span><span class="p">[</span><span class="mi">14</span><span class="p">][</span><span class="s2">&quot;Filename&quot;</span><span class="p">],</span><span class="mi">0</span><span class="p">))</span>
</pre></div>

</div>
</div>
</div>

<div class="output_wrapper">
<div class="output">


<div class="output_area">
<div class="prompt"></div>



<div class="output_png output_subarea ">
<img src="data:image/png;base64,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
wUHju9FCY1iZk0qymsw+KoWhoSElk0k1NTWppKTEhs0QdnP5aTxGZIEzGRkZ0de+9jXLNbg8vWuo
XYWUy0U//fTTOvXUUz3tad3KQddQuny/JFNA9PT0aPXq1cpkMtbjhktSWlpqvfehK2h+VVhYaFwo
6ooZM2aYc0MJhOEdHR21bqyohmpqahQKhVRWVqbPfOYzdsZ4YJQBF3zPVblAZ7FmfE63/cR0Q9fd
3W3TqVzE+/DDD1uiGUqSHizcBRzk0NCQVW2S4IWeo2Ia9C/J2olLU/3imTFBXiedTqu2tlbnnHOO
nV033+AmqV2HyIN71Nvba/fFbezGz7uafTfBDVB47rnn9Oabb5oih4Zzkqxqmir9ZDJp8sfm5mal
UimlUilrybBixQoNDAyYgIK73N7ertWrVyuRSCiXy2nOnDnq6upSKBTSwMCAUabM1ubzU0fQ19en
7u5uzZ492wq9IpGIysvLLZqghqO0tNRoRGhkziRV67lc7uCkdN5+++0bx8fHLYQGYcOBk/wAFXHR
MWagEhQy2WzWDLWbfMWrctloXUvPGGZ+gkgI3d1e3NAoGH4kjX6/3xLNJMrIMfj9+dmtSOVaWlqs
N4iLetyoA+OKoX366af1xBNPWMMwn8+nhoYGZTIZ1dbWamRkRNXV1VZJSMERoTwXhqiJxBLGgK97
enrMWNJ2uqCgQJWVlbriiit09NFHW8EVBxqjP51+QI/c1dWlTZs2qa6uToceeqhFcq7qZDrdw37h
bJCE3nvvvdq+fbsZCDf3QcLrwIEDam5uVjKZVGtrq/bu3WvVpKWlpZaALSkp8VA6yPN4P8Fg0Fpk
YMAKCwv15S9/WfPmzTNQ4Ro0jCEgQpJxu6gytm3bpvr6elsvPi8Olr1yjWV5ebkhc5Dfb37zG4vg
6KUEuCEixoAjQUUNxcwCcg7QDT09PdZOYObMmerr67PZulCcGB/UQytWrJDP57OKWN6fS8G6UQ4P
V0rJbGMiG/h5qBHOhc/nMzpRkt3B5557zsAPAIP7y++gtkPSzR1wpbyDg4N65ZVX1NbWpnA4bPMN
AoGAamtrtWTJEp122mk69NBDtWbNGlVXV3sSwdDIY2Njnp5P8Xjc7BHRC04UwEARKH9gN9wqXibA
8fcZZ5xx8CH8G264IQfKJDQipIE3hTvG8BN6E5bD0bkNpUKhqQlSKBWmN2fCUMHbkojFCJCAGhoa
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
HgAAAABJRU5ErkJggg==
"
>
</div>

</div>

<div class="output_area">
<div class="prompt"></div>



<div class="output_png output_subarea ">
<img src="data:image/png;base64,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
J04gEAggFArh1KlTCAQCePLJJ3HDDTegXC5j+/btGBgYgK7rEl3BZ2ZpZJPJhLfeegvd3d0S7hYK
hWQPUSZhse8YeQQ0ohjSdMz6TSQSIlgoyGZnZxEKhaQEMktjrK+vw2azIRAISCnp+++/f1M+HWie
fage43jqui7OeKMCURekcZEa7/Fu1oN63GiRNDun1T2MP/V6XfxM0WhUHNe0cJnjsb6+Lvs1vP76
6+js7JRY+7GxMcmgptAvFApIp9NYW1vD+vq61CSiv4SgifOfXLvP55O5SXBCZf7YY4/hzJkz+OEP
fyi5FAsLC3jrrbdw6NAhPPPMM+jp6cHly5fh8/ng9XqlkB8336azVdM0fPe730Vvby9CoRDsdrsk
C66traFW26itX61ubHzO4AS73Y729naUSiV0dnYiGo2K9TgzMwOPx4OPfOQj2L9/P06ePInbbrtN
5uJnPvMZXLp0CV6vF3/5l3+JV155Bbt37xYkPjo6ilgsho6ODtx9990NRcqazUPj2FI2HDt2DIuL
i8LR79y5E+fPn6fP6m+aTi5Duy52mMrn8xLqCFytqcFCVsDVMKNyuSzRGOQOX3vtNdx9993YtWsX
bDabbBasaZqUIODEY+Ykf8jJqfQKkTx5cCZaqaV2qXk9Hg8KhYLsExuNRhGNRrG0tIRIJIITJ040
vCtNT1bfq9frgoqY0g1scLehUEiSMCqVCtrb2yWmnEKJrVqt4vXXX5fJsri4KBmydrsdDz/8sCze
W265RUox53I5TE1N4dVXX22IQqjXN0oq2Gw2jI2NCYqjEIxEIrJ9WzKZlOgAOqzJH7JfbTabxGaH
QiHouo4bbrgB9Xod4+PjsFqt2Lt3L+x2O8bHxzE6Oop4PC59VKvVZA9gUgWJREJQKu+l/latRPV/
KqAwmUx44oknJLzX4/Hgn/7pn/BP//RP+MY3vtGwg5b6vVbX3OxerYR5s2akouibaHUNUlx9fX1I
JBIClFjS27idHYWbruuYmprC8vIybDab1MUJBoOSKev3+8Vn1N/fD5PJhKGhIVitVgSDQUlUAiAb
1Oj6RgmTEydOCJWkZrWGQqH/Q917R8dZXtvDe6qk0Yyk0UgjjbpkVctFso27AJtm4xh8wRSTUJKb
GyAFQgg3K5f8ICHJDUkuKeQmK6GFmBaaTYwBG4ObZFywJUuyeq8jadSnSKNp3x9z9/E7whDfP+5a
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
0tLS1NDQIEn2/Gg2Oiu6iooKq0w6OztVU1NjG9MSiYTp0SP2dvHiRetFECid1tjYqIGBgWVZPr7E
aZx5J+mDZ4LiK8+AJe9sR3OuApydndUf/dEf2Wcj+MbjcdNYmp6eXrZYiXeKQJGfn69wOCy3261f
/epX+rM/+zPzdySWTmJHbm6uWltb/x9edbldERn9/fffv+ikSeHUcVREUCeEQ/kCS4AhGjjdwAS8
ZGSpvFhAO86GLxixU1mSTIrsnN9PWctSFLIQqHZAIzTxkDfA8c7OzqqkpEQLC0uLyOkrELighsJO
oUkHDEIpX11dbdkr30dAwhFywGg001SEJx6JROzz4myY4uUFJ6DAlGGJBdkb18bPBYbhZ/L5wNvh
ORNUCLTOQSwgDppSzpKVoISDoLohSMPbhq7HGSLYs8nJCZVJsiyV5GN2dlZFRUUW+D0ej+00AKJD
6C0rK0sFBQUaHR01OIaSnfWDyeTSYgmGu6hK/jvoEn0er9drSYVzHgC8njNKgoSKq9e7tFyde5mf
n28sJYIglfClS5eUl5cnv99vFGZgJ6o95zsFBAkVd3Fx0VY2Li4uGgbOOSAzRZ9pdnZW1dXVOn36
tC2Df/zxxw3yiMViqq6u1qFDh/Thhx8qPz9f69at07XXXquCggI99NBD9rucEO7CwoKeffZZ7d+/
35IE/s3pT6TLO2uBfqhS6Ls4+e5OGje+ykmRJDBK0vnz540dxXCjz+ezatYZnHDiPp/PelOrVq2y
HcecW0QfuRbe2Y8++uiLA92kLGUpS1nK/v+Z+3/+kpSlLGUpS9kX2VKOPmUpS1nKrnJLOfqUpSxl
KbvKLeXoU5aylKXsKreUo09ZylKWsqvcUo4+ZSlLWcqucks5+pSlLGUpu8ot5ehTlrKUpewqt5Sj
T1nKUpayq9xSjj5lKUtZyq5ySzn6lKUsZSm7yi3l6FOWspSl7Cq3lKNPWcpSlrKr3FKOPmUpS1nK
rnJLOfqUpSxlKbvKLeXoU5aylKXsKreUo09ZylKWsqvcUo4+ZSlLWcqucks5+pSlLGUpu8ot5ehT
lrKUpewqt5SjT1nKUpayq9xSjj5lKUtZyq5ySzn6lKUsZSm7yu3/AC3QIgaJdNz8AAAAAElFTkSu
QmCC
"
>
</div>

</div>

<div class="output_area">
<div class="prompt"></div>



<div class="output_png output_subarea ">
<img src="data:image/png;base64,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
"
>
</div>

</div>

<div class="output_area">
<div class="prompt"></div>



<div class="output_png output_subarea ">
<img src="data:image/png;base64,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
vfaaiEx5+LgWxzGcVs7EQs8vD0MhviIMDOH4uNmcNwTC31l/AL73hbxbBghkHqwyD4/H/z7fgK+9
dgNB3zlNK9fZxMSwnG68OPiu0+moqbi3t6cTlQN3OE4BLzOHwvM4ctDGSbCJhVx8izDEfud7xvMm
uW9D820a9rMNrjrpPHssKxHMGmPohceDcwXxgmuFGVIM+UwmE+e9wPV5oecNwFc6EsLt5YIk/A5y
e/gdsVAqz4Visahzj1k+kEajofMwn88774LIdPEGBh1FkS7q8XhcMXQseolEQvn5o9FIoZlaraab
AvrY7/cVeqxUKs55gGaw4B8dHcmdO3dEZBroBbbZYDDQtASAfvBO4lrox+9+9zt57733tD0QPO+b
N2/qc9vf31cfBvtg4CfjYi8vvfSSN9DzJAnQTZAgQYKcczkTGr2PocISi8WcogYQDu1nLccWKfFF
P+J+PtPcp7H7tEgfnIN7WKcpRy+yduqDmlhrs85hZtWg76xR4rqtVmsm2q/dbmvJs2KxqCYvnECs
hTLcwtfwMaT6/b5j9vuOfZY2Ps8BflLEs+/58bkniQ8qYv44y0lOYAsj+e7LLDC2cpiJYYuOp9Np
1fy474lEwomqxnVZuz8JwuJrWSjRJsFj9hezdWzpQrSF86njOUZRNPPsk8mkzt8oimags3q9rnOy
XC47ECE0W7SxUCjIiy++qG1A1Go8HldLF/DSZHJcGKbZbCo8VCgUZphBnP1yf3/fidpFG1CAZDKZ
OMVHYB2Mx2OFRQED3bp1y2Hz4B2tVCoKO6FdyWRS29BoNJz8+bA8TitnYqH3UQ25iAZDCljw4/G4
mj0Wv+WgBxE3iGQwGOgiarFj/LVBMfy7ba8PnrB0Qiv8YtpAHtsGtBP3wObGZjbDCfhcr9cVV2Tc
HhM9n8/r5MPE499zuZy2h0PSgWu22+2Zcbbi2yjn+UDmba4MkfjgNz7vJPYSi4+qyW3zMXns9XyL
P6eYYHiDz/VlV43FYs6cFHGpjXaTY6YMBNflvDk8P/m+7PfgDQLPFgv2cDh0NhX8zllOeZPkxRDX
3dvb075woRp8t7W1pQsjoMRGo6GQBbeBqZi8wXCeGkAkvV5P5zV+v3//vuLgy8vLWpiEWUK4782b
N3Ux3dnZkddff137ANgIm1Gn01E4B+0XmRZCwWaAjeDixYva9idPnshvf/tb/R6bF1Nt4Q/Y2trS
5729vS2ffvqpiIj86Z/+qZxGAnQTJEiQIOdcFn5f722QIEGCBPm/JUGjDxIkSJBzLmGhDxIkSJBz
LmGhDxIkSJBzLmGhDxIkSJBzLmGhDxIkSJBzLmGhDxIkSJBzLmGhDxIkSJBzLmGhDxIkSJBzLmGh
DxIkSJBzLmGhDxIkSJBzLmGhDxIkSJBzLmGhDxIkSJBzLmGhDxIkSJBzLmGhDxIkSJBzLmGhDxIk
SJBzLmGhDxIkSJBzLmGhDxIkSJBzLmGhDxIkSJBzLmGhDxIkSJBzLmGhDxIkSJBzLmGhDxIkSJBz
LmGhDxIkSJBzLmGhDxIkSJBzLv8fSTGgrHazEicAAAAASUVORK5CYII=
"
>
</div>

</div>

</div>
</div>

</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[52]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="n">external_testing_labels</span> <span class="o">=</span> <span class="p">(</span><span class="n">np</span><span class="o">.</span><span class="n">int32</span><span class="p">(</span><span class="n">df_external_testing</span><span class="p">[</span><span class="s2">&quot;label&quot;</span><span class="p">]</span><span class="o">.</span><span class="n">values</span><span class="p">))</span><span class="o">.</span><span class="n">flatten</span><span class="p">()</span>
<span class="n">external_testing_data</span> <span class="o">=</span> <span class="n">df_compute_histograms_parallel</span><span class="p">(</span><span class="n">df_external_testing</span><span class="p">)</span>
</pre></div>

</div>
</div>
</div>

<div class="output_wrapper">
<div class="output">


<div class="output_area">
<div class="prompt"></div>

<div class="output_subarea output_stream output_stdout output_text">
<pre>Slice:  5
Progress(Start)-&gt;  0| 16 T- 19: 11: 12
Progress-&gt;  1|1 T- 19: 11: 12
Progress-&gt;  5|5 T- 19: 11: 23
</pre>
</div>
</div>

<div class="output_area">
<div class="prompt"></div>

<div class="output_subarea output_stream output_stderr output_text">
<pre>[Parallel(n_jobs=4)]: Done   2 out of   4 | elapsed:   11.9s remaining:   11.9s
</pre>
</div>
</div>

<div class="output_area">
<div class="prompt"></div>

<div class="output_subarea output_stream output_stdout output_text">
<pre>Progress-&gt;  5|5 T- 19: 11: 26
Progress-&gt;  5|5 T- 19: 11: 27
Progress-&gt;  16|16 T- 19: 11: 27
</pre>
</div>
</div>

<div class="output_area">
<div class="prompt"></div>

<div class="output_subarea output_stream output_stderr output_text">
<pre>[Parallel(n_jobs=4)]: Done   4 out of   4 | elapsed:   15.3s finished
</pre>
</div>
</div>

</div>
</div>

</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[&nbsp;]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span> 
</pre></div>

</div>
</div>
</div>

</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[87]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="c1"># Define a model to predict with</span>
<span class="n">rf</span> <span class="o">=</span> <span class="n">ensemble</span><span class="o">.</span><span class="n">RandomForestClassifier</span><span class="p">(</span><span class="n">n_estimators</span><span class="o">=</span><span class="mi">64</span><span class="p">,</span><span class="n">n_jobs</span><span class="o">=</span><span class="mi">4</span><span class="p">)</span>
<span class="n">predictor</span> <span class="o">=</span> <span class="n">rf</span>

<span class="c1"># Train a model with the training data</span>
<span class="o">%</span><span class="k">time</span> predictor.fit(data_histograms, data_labels)

<span class="c1"># Predict the test data&#39;s labels</span>
<span class="n">predicted_labels</span> <span class="o">=</span> <span class="n">predictor</span><span class="o">.</span><span class="n">predict</span><span class="p">(</span><span class="n">external_testing_data</span><span class="p">)</span>

<span class="nb">print</span><span class="p">(</span><span class="s2">&quot;Test predictions accuracy:&quot;</span><span class="p">,</span><span class="n">predictor</span><span class="o">.</span><span class="n">score</span><span class="p">(</span><span class="n">external_testing_data</span><span class="p">,</span><span class="n">external_testing_labels</span><span class="p">))</span>
</pre></div>

</div>
</div>
</div>

<div class="output_wrapper">
<div class="output">


<div class="output_area">
<div class="prompt"></div>

<div class="output_subarea output_stream output_stderr output_text">
<pre>/usr/local/lib/python3.5/dist-packages/ipykernel/__main__.py:1: DataConversionWarning: A column-vector y was passed when a 1d array was expected. Please change the shape of y to (n_samples,), for example using ravel().
  if __name__ == &#39;__main__&#39;:
</pre>
</div>
</div>

<div class="output_area">
<div class="prompt"></div>

<div class="output_subarea output_stream output_stdout output_text">
<pre>CPU times: user 10.9 s, sys: 0 ns, total: 10.9 s
Wall time: 3.55 s
Test predictions accuracy: 0.3125
</pre>
</div>
</div>

</div>
</div>

</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[66]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="c1"># Define a model to predict with</span>
<span class="n">mlpc</span> <span class="o">=</span> <span class="n">MLPClassifier</span><span class="p">(</span><span class="n">max_iter</span><span class="o">=</span><span class="mi">1600</span><span class="p">,</span><span class="n">hidden_layer_sizes</span><span class="o">=</span><span class="p">(</span><span class="mi">1000</span><span class="p">,</span><span class="mi">400</span><span class="p">),</span><span class="n">shuffle</span><span class="o">=</span><span class="kc">True</span><span class="p">,</span> <span class="n">random_state</span><span class="o">=</span><span class="mi">42</span><span class="p">)</span>
<span class="n">predictor</span> <span class="o">=</span> <span class="n">mlpc</span>

<span class="c1"># Train a model with the training data</span>
<span class="o">%</span><span class="k">time</span> predictor.fit(data_histograms, data_labels)

<span class="c1"># Predict the test data&#39;s labels</span>
<span class="n">predicted_labels</span> <span class="o">=</span> <span class="n">predictor</span><span class="o">.</span><span class="n">predict</span><span class="p">(</span><span class="n">external_testing_data</span><span class="p">)</span>

<span class="nb">print</span><span class="p">(</span><span class="s2">&quot;Test predictions accuracy:&quot;</span><span class="p">,</span><span class="n">predictor</span><span class="o">.</span><span class="n">score</span><span class="p">(</span><span class="n">external_testing_data</span><span class="p">,</span><span class="n">external_testing_labels</span><span class="p">))</span>
</pre></div>

</div>
</div>
</div>

<div class="output_wrapper">
<div class="output">


<div class="output_area">
<div class="prompt"></div>

<div class="output_subarea output_stream output_stderr output_text">
<pre>/usr/local/lib/python3.5/dist-packages/sklearn/neural_network/multilayer_perceptron.py:904: DataConversionWarning: A column-vector y was passed when a 1d array was expected. Please change the shape of y to (n_samples, ), for example using ravel().
  y = column_or_1d(y, warn=True)
</pre>
</div>
</div>

<div class="output_area">
<div class="prompt"></div>

<div class="output_subarea output_stream output_stdout output_text">
<pre>CPU times: user 9min 21s, sys: 9min 47s, total: 19min 9s
Wall time: 5min 20s
Test predictions accuracy: 0.375
</pre>
</div>
</div>

</div>
</div>

</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[71]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="c1"># Define a model to predict with</span>
<span class="n">predictor</span> <span class="o">=</span> <span class="n">cv2</span><span class="o">.</span><span class="n">ml</span><span class="o">.</span><span class="n">SVM_create</span><span class="p">();</span>

<span class="n">predictor</span><span class="o">.</span><span class="n">setKernel</span><span class="p">(</span><span class="n">cv2</span><span class="o">.</span><span class="n">ml</span><span class="o">.</span><span class="n">SVM_RBF</span><span class="p">)</span>
<span class="n">predictor</span><span class="o">.</span><span class="n">setType</span><span class="p">(</span><span class="n">cv2</span><span class="o">.</span><span class="n">ml</span><span class="o">.</span><span class="n">SVM_C_SVC</span><span class="p">)</span>
<span class="c1">#predictor.setC(62.5)</span>
<span class="c1">#predictor.setGamma(0.50625)</span>

<span class="c1"># Train a model with the training data</span>
<span class="o">%</span><span class="k">time</span> predictor.train(data_histograms, cv2.ml.ROW_SAMPLE, data_labels)

<span class="c1"># Predict the test data&#39;s labels</span>
<span class="n">retval</span><span class="p">,</span> <span class="n">predicted_labels</span> <span class="o">=</span> <span class="n">predictor</span><span class="o">.</span><span class="n">predict</span><span class="p">(</span><span class="n">external_testing_data</span><span class="p">)</span>
</pre></div>

</div>
</div>
</div>

<div class="output_wrapper">
<div class="output">


<div class="output_area">
<div class="prompt"></div>

<div class="output_subarea output_stream output_stdout output_text">
<pre>CPU times: user 51.7 s, sys: 36 ms, total: 51.7 s
Wall time: 51.9 s
</pre>
</div>
</div>

</div>
</div>

</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[88]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="c1"># Flatten the Nsamples,1 matrix into a Nsamples array</span>
<span class="n">predicted_labels</span> <span class="o">=</span> <span class="n">np</span><span class="o">.</span><span class="n">int32</span><span class="p">(</span><span class="n">predicted_labels</span><span class="o">.</span><span class="n">flatten</span><span class="p">())</span>

<span class="c1"># compute the confusion matrix</span>
<span class="n">conf_matrix</span> <span class="o">=</span> <span class="n">compute_confusion_matrix</span><span class="p">(</span><span class="n">external_testing_labels</span><span class="p">,</span> <span class="n">predicted_labels</span><span class="p">)</span>
</pre></div>

</div>
</div>
</div>

</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[89]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="c1"># Evaluate the results</span>
<span class="nb">print</span><span class="p">(</span><span class="s2">&quot;Total samples-&gt;&quot;</span><span class="p">,</span><span class="nb">len</span><span class="p">(</span><span class="n">data_frame</span><span class="p">))</span>
<span class="nb">print</span><span class="p">(</span><span class="s2">&quot;Used samples(more than 0 kp detected)-&gt;&quot;</span><span class="p">,</span> <span class="nb">len</span><span class="p">(</span><span class="n">df</span><span class="p">))</span>
<span class="nb">print</span><span class="p">(</span><span class="s2">&quot;Discarded samples(0 kp detected)-&gt;&quot;</span><span class="p">,</span> <span class="nb">len</span><span class="p">(</span><span class="n">data_frame</span><span class="p">)</span><span class="o">-</span><span class="nb">len</span><span class="p">(</span><span class="n">df</span><span class="p">))</span>
<span class="nb">print</span><span class="p">(</span><span class="s2">&quot;----------------------------------------------------------------------&quot;</span><span class="p">)</span>
<span class="nb">print</span><span class="p">(</span><span class="s2">&quot;BOW size-&gt;&quot;</span><span class="p">,</span> <span class="n">n_centroids</span><span class="p">,</span><span class="s2">&quot;words&quot;</span><span class="p">)</span>
<span class="nb">print</span><span class="p">(</span><span class="s2">&quot;----------------------------------------------------------------------&quot;</span><span class="p">)</span>
<span class="nb">print</span><span class="p">(</span><span class="s2">&quot;Training/Test split ratio-&gt;&quot;</span><span class="p">,</span> <span class="n">split_ratio</span><span class="p">)</span>
<span class="nb">print</span><span class="p">(</span><span class="s2">&quot;Training samples-&gt;&quot;</span><span class="p">,</span> <span class="nb">len</span><span class="p">(</span><span class="n">training_labels</span><span class="p">))</span>
<span class="nb">print</span><span class="p">(</span><span class="s2">&quot;Test samples-&gt;&quot;</span><span class="p">,</span><span class="nb">len</span><span class="p">(</span><span class="n">test_labels</span><span class="p">))</span>
<span class="nb">print</span><span class="p">(</span><span class="s2">&quot;----------------------------------------------------------------------&quot;</span><span class="p">)</span>
<span class="c1">#print(&quot;Model used-&gt;&quot;, predictor.getDefaultName())</span>
<span class="nb">print</span><span class="p">(</span><span class="s2">&quot;Test right predictions-&gt;&quot;</span><span class="p">,</span> <span class="n">conf_matrix</span><span class="o">.</span><span class="n">trace</span><span class="p">(),</span><span class="s2">&quot;|&quot;</span><span class="p">,</span><span class="nb">str</span><span class="p">(</span><span class="n">conf_matrix</span><span class="o">.</span><span class="n">trace</span><span class="p">()</span><span class="o">/</span><span class="n">np</span><span class="o">.</span><span class="n">sum</span><span class="p">(</span><span class="n">conf_matrix</span><span class="p">,(</span><span class="mi">0</span><span class="p">,</span><span class="mi">1</span><span class="p">))</span><span class="o">*</span><span class="mi">100</span><span class="p">)</span><span class="o">+</span><span class="s2">&quot;%&quot;</span><span class="p">)</span>
<span class="nb">print</span><span class="p">(</span><span class="s2">&quot;Test wrong predictions-&gt;&quot;</span><span class="p">,</span> <span class="n">np</span><span class="o">.</span><span class="n">sum</span><span class="p">(</span><span class="n">conf_matrix</span><span class="p">,(</span><span class="mi">0</span><span class="p">,</span><span class="mi">1</span><span class="p">))</span><span class="o">-</span><span class="n">conf_matrix</span><span class="o">.</span><span class="n">trace</span><span class="p">(),</span><span class="s2">&quot;|&quot;</span><span class="p">,</span><span class="nb">str</span><span class="p">((</span><span class="mi">1</span><span class="o">-</span><span class="n">conf_matrix</span><span class="o">.</span><span class="n">trace</span><span class="p">()</span><span class="o">/</span><span class="n">np</span><span class="o">.</span><span class="n">sum</span><span class="p">(</span><span class="n">conf_matrix</span><span class="p">,(</span><span class="mi">0</span><span class="p">,</span><span class="mi">1</span><span class="p">)))</span><span class="o">*</span><span class="mi">100</span><span class="p">)</span><span class="o">+</span><span class="s2">&quot;%&quot;</span><span class="p">)</span>
</pre></div>

</div>
</div>
</div>

<div class="output_wrapper">
<div class="output">


<div class="output_area">
<div class="prompt"></div>

<div class="output_subarea output_stream output_stdout output_text">
<pre>Total samples-&gt; 7855
Used samples(more than 0 kp detected)-&gt; 7023
Discarded samples(0 kp detected)-&gt; 832
----------------------------------------------------------------------
BOW size-&gt; 1024 words
----------------------------------------------------------------------
Training/Test split ratio-&gt; 0.9
Training samples-&gt; 6320
Test samples-&gt; 703
----------------------------------------------------------------------
Test right predictions-&gt; 5 | 31.25%
Test wrong predictions-&gt; 11 | 68.75%
</pre>
</div>
</div>

</div>
</div>

</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[90]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="nb">print</span><span class="p">(</span><span class="s2">&quot;Confusion Matrix:&quot;</span><span class="p">)</span>
<span class="n">actual</span> <span class="o">=</span> <span class="n">pd</span><span class="o">.</span><span class="n">Series</span><span class="p">(</span><span class="n">external_testing_labels</span><span class="p">,</span> <span class="n">name</span><span class="o">=</span><span class="s2">&quot;Actual&quot;</span><span class="p">)</span>

<span class="n">predicted</span> <span class="o">=</span> <span class="n">pd</span><span class="o">.</span><span class="n">Series</span><span class="p">(</span><span class="n">predicted_labels</span><span class="p">,</span> <span class="n">name</span><span class="o">=</span><span class="s2">&quot;Real\Predicted&quot;</span><span class="p">)</span>
<span class="n">confmat</span> <span class="o">=</span> <span class="n">pd</span><span class="o">.</span><span class="n">crosstab</span><span class="p">(</span><span class="n">actual</span><span class="p">,</span> <span class="n">predicted</span><span class="p">)</span>

<span class="n">confmat</span><span class="o">.</span><span class="n">columns</span> <span class="o">=</span> <span class="n">sign_categories</span><span class="o">.</span><span class="n">categories</span><span class="p">[</span><span class="n">np</span><span class="o">.</span><span class="n">int32</span><span class="p">(</span><span class="n">confmat</span><span class="o">.</span><span class="n">columns</span><span class="p">)]</span>
<span class="n">confmat</span> <span class="o">=</span> <span class="n">confmat</span><span class="o">.</span><span class="n">set_index</span><span class="p">(</span><span class="n">sign_categories</span><span class="o">.</span><span class="n">categories</span><span class="p">[</span><span class="n">confmat</span><span class="o">.</span><span class="n">index</span><span class="p">])</span>
<span class="nb">print</span><span class="p">(</span><span class="n">confmat</span><span class="p">)</span>
</pre></div>

</div>
</div>
</div>

<div class="output_wrapper">
<div class="output">


<div class="output_area">
<div class="prompt"></div>

<div class="output_subarea output_stream output_stdout output_text">
<pre>Confusion Matrix:
                    stop
noLeftTurn             1
pedestrianCrossing     3
school                 4
speedLimit25           1
stop                   5
stopAhead              1
yield                  1
</pre>
</div>
</div>

</div>
</div>

</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[91]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="k">def</span> <span class="nf">compute_precision</span><span class="p">(</span><span class="n">c_mat</span><span class="p">):</span>
    <span class="c1"># Reduce by cols</span>
    <span class="n">totals</span> <span class="o">=</span> <span class="n">np</span><span class="o">.</span><span class="n">sum</span><span class="p">(</span><span class="n">c_mat</span><span class="p">,</span><span class="mi">0</span><span class="p">)</span>
    
    <span class="n">totals</span> <span class="o">=</span> <span class="n">np</span><span class="o">.</span><span class="n">float32</span><span class="p">(</span><span class="n">totals</span><span class="p">)</span>
    <span class="c1"># Mask rows that have no values</span>
    <span class="n">mask</span> <span class="o">=</span> <span class="n">totals</span><span class="o">&gt;</span><span class="mi">0</span>
    
    <span class="k">for</span> <span class="n">ii</span> <span class="ow">in</span> <span class="nb">range</span><span class="p">(</span><span class="nb">len</span><span class="p">(</span><span class="n">totals</span><span class="p">)):</span>
        <span class="k">if</span><span class="p">(</span><span class="n">totals</span><span class="p">[</span><span class="n">ii</span><span class="p">]</span> <span class="o">&gt;</span> <span class="mi">0</span><span class="p">):</span>
            <span class="n">totals</span><span class="p">[</span><span class="n">ii</span><span class="p">]</span> <span class="o">=</span> <span class="n">c_mat</span><span class="p">[</span><span class="n">ii</span><span class="p">,</span><span class="n">ii</span><span class="p">]</span> <span class="o">/</span> <span class="n">totals</span><span class="p">[</span><span class="n">ii</span><span class="p">]</span>
            <span class="k">pass</span>
    
    
    <span class="c1"># Drop rows that have no values (no test instances)</span>
    <span class="k">return</span> <span class="n">totals</span><span class="p">[</span><span class="n">mask</span><span class="p">]</span>
</pre></div>

</div>
</div>
</div>

</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[92]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="c1"># This means that for each time that the predicted label is one of these</span>
<span class="c1"># the predictor was right in the corresponding fraction of predictions (TP/TP+FP)</span>
<span class="n">precision</span> <span class="o">=</span> <span class="n">pd</span><span class="o">.</span><span class="n">DataFrame</span><span class="p">(</span><span class="n">compute_precision</span><span class="p">(</span><span class="n">conf_matrix</span><span class="p">),</span> <span class="n">columns</span><span class="o">=</span><span class="p">[</span><span class="s2">&quot;Prediction precision&quot;</span><span class="p">])</span>
<span class="n">precision</span> <span class="o">=</span> <span class="n">precision</span><span class="o">.</span><span class="n">set_index</span><span class="p">(</span><span class="n">confmat</span><span class="o">.</span><span class="n">columns</span><span class="p">)</span>

<span class="nb">print</span><span class="p">(</span><span class="n">precision</span><span class="p">)</span>
</pre></div>

</div>
</div>
</div>

<div class="output_wrapper">
<div class="output">


<div class="output_area">
<div class="prompt"></div>

<div class="output_subarea output_stream output_stdout output_text">
<pre>      Prediction precision
stop                0.3125
</pre>
</div>
</div>

</div>
</div>

</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[&nbsp;]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span> 
</pre></div>

</div>
</div>
</div>

</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[&nbsp;]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span> 
</pre></div>

</div>
</div>
</div>

</div>
    </div>
  </div>
</body>

 


</html>


{:/}
