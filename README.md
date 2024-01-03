<body><h1>αβ-ProtBERT</h1>
<br>
<p>
Current version of the JLPrice lab ProtBERT implementation. Modified and in-testing to process batches of sequences.
<br><br>
A simple implementation of BERT for multiple-mask protein predictions for use with Google Colab.
This implementation uses the <a href=https://huggingface.co/Rostlab/prot_bert>prot_bert</a> pretrained model.<br>
<br>
Eventually a full python implementation will be made for usage on local or remote machines.
<br>
This implementation supports both fine-tuning and multiple-mask MLM predictions.<hr>
<br>
Refer to this README for running instructions and for an explanation of all various settings available.
<hr>
<h2>Configuration_Settings</h2>
<p>These settings relate to how the program will be run.</p>
<h3>use_custom_weights</h3>
<p>This setting specifies whether or not custom weights should be used. The following are allowed values for this setting:</p>
<ul>
    <li><strong>"No"</strong> : Use default weights</li>
    <li><strong>"From Computer"</strong> : Use a custom weights file from your computer</li>
    <li><strong>"From Google Drive or Colab"</strong> : Use a custom weights file from your Google Drive or your current Colab session</li>
</ul>
<h3>path_to_file_in_google</h3>
<p>This setting is only used if <code>use_custom_weights</code> is set to <code>"From Google Drive or Colab</code> and specifies where a weights file can be found.
The required file path can be most easily obtained by running the first cell of the main program titled <code>Setup, Imports, and Connect to Runtime</code> to mount your drive
 and then navigating the Colab file explorer to find your file, right-clicking it, and then selecting "Copy as path".</p>
<h3>fine_tune_model</h3>
<p>This setting specifies whether or not the model should be fine tuned before performing an MLM prediction session. Note that this can take several hours to complete. Acceptable values are (without quotation marks):</p>
<ul>
    <li><strong>true</strong> : Perform fine tuning session</li>
    <li><strong>false</strong> : Skip fine tuning session</li>
</ul>
<h3>MLM_prediction</h3>
<p>This setting specifies whether or not the model should perform an MLM prediction session. Acceptable values are (without quotation marks):</p>
<ul>
    <li><strong>true</strong> : Perform an MLM prediction session</li>
    <li><strong>false</strong> : Skip MLM prediction session</li>
</ul>
<hr>
<h2>Fine_Tune_Settings</h2>
<p>These settings are only used if <code>fine_tune_model</code> is set to <code>true</code></p>
<p>-------------------------------------------------------------------------------------------------------------</p>
<h3>FT_Settings</h3>
<p>-------------------------------------------------------------------------------------------------------------</p>
<h3>fine_tune_file_source</h3>
<p>This setting specifies the source where the fine tune file can be found. The fine tuning file should be a <code>.txt</code> file, organized
with one full protein/peptide sequence per line. <a href="https://github.com/cathepsin/protein_BERT_DATASETS">Example fine tuning files can be found here</a>.
Allowed values are the following:</p>
<ul>
    <li><strong>"From Computer"</strong> : Use a fine tune file from your computer</li>
    <li><strong>"From Google Drive or Colab"</strong> : Use a fine tune file from your Google Drive or your current Colab session</li>
</ul>

<h3>path_to_fine_tune_file</h3>
<p>This setting is only used if <code>fine_tune_file_source</code> is set to <code>"From Google Drive or Colab</code> and specifies where a fine tune file can be found.
The required file path can be most easily obtained by running the first cell of the main program titled <code>Setup, Imports, and Connect to Runtime</code> to mount your drive
 and then navigating the Colab file explorer to find your file, right-clicking it, and then selecting "Copy as path".</p>

<h3>percent_to_mask</h3>
<p>This setting specifies what percentage of tokens to mask during fine tuning. A suggested default from the
<a href="https://arxiv.org/abs/1810.04805">original BERT paper</a> is <strong>0.15</strong>. This value should be a real number
between 0 and 1, and requires a leading <code>0</code> before the decimal point.</p>

<h3>max_length</h3>
<p>This setting specifies the maximum length of a protein sequence to view during fine tuning. If a sequence is
encountered that exceeds the specified maximum length, the sequence will be truncated. This value should be an integer.</p>

<h3>batch_size</h3>
<p>This setting specifies how many groups your set of sequences provided in your fine tune file should be organized into. A more 
rigorous understanding of how batch sizes can alter fine tuning can be found elsewhere. In short, though, a higher batch size results in
faster and more course training, while smaller batch sizes result in the opposite.</p>

<h3>number_of_epochs</h3>
<p>This setting specifies how many times the fine tune file should be processed. Time required to complete fine tuning increases
<em>linearly</em> with the number of epochs. More epochs typically results in higher training accuracy, but can result in overfitting
a model if done in excess. Consult other sources for a more rigorous understanding of how to select a correct number of epochs to avoid
overfitting a model. A rather safe number (though admittedly naive to specific fine tune files) is around 100.</p>

<h3>learning_rate</h3>
<p>This setting controls how swiftly the model can be fine tuned. For a comprehensive, but somewhat esoteric, description of how learning rates
can alter fine tuning, <a href="https://pytorch.org/docs/stable/generated/torch.optim.Adam.html">conslut the optimizer documentation.</a> In short,
a higher learning rate causes the model to fine tune in more drastic and "grainy" steps, while a smaller value causes smaller and more "fine" steps.
A value around <strong>0.01</strong> is considered very large, and a value of <strong>0.00001</strong> is considered very small.</p>
<p>-------------------------------------------------------------------------------------------------------------</p>
<h3>Save_Settings</h3>
<p>-------------------------------------------------------------------------------------------------------------</p>
<h3>save_every_number_of_steps</h3>
<p>This setting specifies how often to save the progress of the fine tuning process. Since fine tuning can often take several hours,
this setting can allow for halting and starting-later fine tuning. Note that this setting refers to the number of <strong>steps</strong>,
not the number of <strong>epochs</strong> completed. Each epoch can have a variable number of steps determined by the batch size. This
value should be an integer. If <code>0</code> is used, then the state will only be saved at the end of the session.</p>

<h3>save_final_to_drive</h3>
<p>This setting specifies whether or not the final state should be saved to your Google drive. Note that a weights file is around 1.6 gigabytes.
The recommended way to save a weights file is to save it first to your Google Drive, download that file <em>from your Drive</em> to your
computer, and then delete it from your Drive to save space. Accepted values are:</p>
<ul>
    <li><strong>true</strong> : Save final state</li>
    <li><strong>false</strong> : Skip final state</li>
</ul>

<h3>download_final_state</h3>
<p>This setting specifies whether or not to directly download the final state to your computer. Note that a weights file is around
1.6 gigabytes. Though this feature is available, it is <em>not recommended</em> due to oddities in how Google Colab downloads files compared
to how it simply saves files to Google Drive.</p>
<hr>
<h2>MLM_Settings</h2>
<p>-------------------------------------------------------------------------------------------------------------</p>
<h3>Sequence_Settings</h3>
<p>-------------------------------------------------------------------------------------------------------------</p>
<h3>process_batch</h3>
<p>This setting specifies whether one single sequence or a batch of sequences should be processed.
If a batch is processed then <code>mask_all_residues</code> (below) will be set to <code>true</code>.
Accepted values are:</p>
<ul>
    <li><strong>true</strong> : Process multiple sequences</li>
    <li><strong>false</strong> : Process only one sequence</li>
</ul>
<p>A batch file should be a <code>.txt</code> file and contain one full sequence on a line, like in the following example:</p>
<pre>
batch.txt
   PSEPHNHDWI
   IPNCPSHREM
   TMANEEHMEK
   GFYCVDYNYI
   ...
</pre>
<p>This setting also takes precedence over all other sequence settings that may modify what sequence or where
a sequence is processed.</p>
<h3>path_to_batch</h3>
<p>This setting is only checked if <code>process_batch</code> is set to <code>true</code>. This specifies the path in
Google Colab or Google Drive to your batch file. The required file path can be most easily obtained by running the first 
cell of the main program titled <code>Setup, Imports, and Connect to Runtime</code> to mount your drive
 and then navigating the Colab file explorer to find your file, right-clicking it, and then selecting "Copy as path".
Eventually there will be an upload-from-computer option implemented.</p>

<h3>use_random_sequence</h3>
<p>This setting specifies that instead of a provided sequence, a random sequence should be generated. This sequence is
generated by randomly sampling <em>n</em> amino acids from the natural 20, each with equal selection probability. Eventually,
this will be modified to use the <a href="https://pypi.org/project/apalib/">apalib</a> random protein generator,
in order to allow for smarter generating. Accepted values are:</p>
<ul>
    <li><strong>true</strong> : Use a random sequence</li>
    <li><strong>false</strong> : Use provided sequence</li>
</ul>
<p>This setting takes precedence over a provided sequence. Arbitrarily, it does NOT take precedence over processing a batch.</p>

<h3>random_sequence_length</h3>
<p>This setting specifies how many residues a random sequence should be. The value should be an integer.</p>

<h3>original_sequence</h3>
<p>This setting is used to specify a sequence to process. This sequence is only used if <code>process_batch</code> and <code>
use_random_sequence</code> are both <code>false</code>. The sequence should contain only the standard
20 amino acid 1-letter codes with no spaces and wrapped in quotations. For example: <code>"PSEPHNHDWIPNCPSHREM"</code> would be a
valid input, while <code>PSEPHNHDWIPNCPSHREM</code>, <code>psephnhdwipncpshrem</code>, and <code>"P S E P H N H D W I P N C P S H R E M"</code>
would all be invalid.</p>

<h3>masked_sequence</h3>
<p>This setting is used to specify which residues in the <code>original_sequence</code> to mask and replace.
The provided sequence should be a copy of the original sequence with the residues to replace switched with either
a lowercase x <code>x</code> or an underscore <code>_</code>. Only those residues that are switched will be 
replaced through the MLM session. For example, if 10 central residues were to be replaced in a sequence, you
might input the following:</p>
<ul>
    <li><strong>original_sequence</strong> : "PSEPHNHDWIPNCPSHREM"</li>
    <li><strong>masked_sequence</strong>   : "PSEP__________SHREM" if using underscores</li>
    <li><strong>masked_sequence</strong>   : "PSEPxxxxxxxxxxSHREM" if using x's</li>
</ul>

<h3>mask_all_residues</h3>
<p>This setting overrides <code>masked_sequence</code> and instead causes the entire provided sequence
to be considered for replacement. Acceptable values are</p>
<ul>
    <li><strong>true</strong> : Consider all residues for replacement</li>
    <li><strong>false</strong> : Use the provided <code>masked_sequence</code></li>
</ul>
<p>This is equivalent to inputting a <code>masked_sequence</code> with all residues masked, as in the following:</p>
<ul>
    <li><strong>original_sequence</strong> : "PSEPHNHDWIPNCPSHREM"</li>
    <li><strong>masked_sequence</strong>   : "___________________" if using underscores</li>
    <li><strong>masked_sequence</strong>   : "xxxxxxxxxxxxxxxxxxx" if using x's</li>
</ul>

<p>-------------------------------------------------------------------------------------------------------------</p>
<h3>Acceptance_Settings</h3>
<p>-------------------------------------------------------------------------------------------------------------</p>
<h3>deterministic</h3>
<p>This setting is used especially for debugging or producing reproducible results (such as those to be published).
For some settings there is some randomness involved. This setting causes a reproducible pseudorandom result instead.
Acceptible values are:</p>
<ul>
    <li><strong>true</strong> : Use a deterministic (reproducible) random number generation state</li>
    <li><strong>false</strong> : Generate results using non-reproducible randomness</li>
</ul>

<h3>deterministic_seed</h3>
<p>This setting is used if <code>deterministic</code> is set to <code>true</code>. This value sets the
starting position for the random number generator. For a more robust description of what a random seed is for those
intrigued, I recommend searching other sources and seeking an understanding for how random numbers are
generated. This value should be an integer or real number.</p>

<h3>slowdown</h3>
<p>Slow down the number of possible replacements when <code>use_threshold</code> is set to <code>false</code>.
This results in only allowing <code>num_changes_to_keep</code> modifications to be made. Accepted values are the following:</p>
<ul>
    <li><strong>true</strong> : Limit the number of replacements allowed in non-thresholded runs</li>
    <li><strong>false</strong> : Allow unlimited modifications in non-threhsolded runs.</li>
</ul>

<h3>use_threshold</h3>
<p>This setting specifies whether replacements should be made using a threshold-percent replacement method or through
random sampling using calculated BERT confidence scores. Accepted values are the following:</p>
<ul>
    <li><strong>true</strong> : Use a threshold replacement method. The semantics of a threshold replacement method can
be found elsewhere in this git repository.</li>
    <li><strong>false</strong> : Make changes by accepting or rejecting a possible change to each individual amino acid
using the calculated BERT confidence scores as the probability for acceptance. This typically results in the inability
to converge to a single solution, and instead run for <code>max_num_iterations</code> iterations. The semantics of a
random replacement method can be found elsewhere in this git repository.</li>
</ul>

<h3>threshold_percent</h3>
<p>This setting specifies the minimum BERT confidence score for a possible replacement. This setting is only
used if <code>use_threshold</code> is set to <code>true</code>. This value must be a real number between 0 and 1. For a well-tuned
model, values below <strong>0.90</strong> often result in non convergence, and values above <strong>0.999</strong> often result in very
few changes being made. A value between <strong>0.99</strong> and <strong>0.995</strong> is suggested.</p>

<h3>num_changes_to_keep</h3>
<p>This setting specifies the maximum number of changes allowed during an iteration for a threshold run, or a random selection
run when <code>slowdown</code> is set to <code>true</code>.</p>

<h3>max_num_iterations</h3>
<p>This setting specifies the maximum number of iterations allowed if no exit or convergence conditions are met. This value should be an integer.</p>

<p>-------------------------------------------------------------------------------------------------------------</p>
<h3>Burn-in_Settings</h3>
<p>-------------------------------------------------------------------------------------------------------------</p>
<h3>iterations_to_burnin</h3>
<p>This setting specifies how many iterations to burn in, or have much looser acceptance settings. This value should be an integer.
To have no burn in iterations, a value of <strong>0</strong> should be used.</p>

<h3>burn_threshold</h3>
<p>This setting specifies the acceptance threshold to use during burn in iterations. This value should be a real number between <strong>0</strong>
and <strong>1</strong>, and must be less than whatever value is used for <code>threshold_percent</code></p>

<h3>burn_num_changes_to_keep</h3>
<p>This setting specifies the number of changes to allow during burn in iterations. This value can be any integer
and should be greater than <code>num_changes_to_keep</code>.</p>

<p>-------------------------------------------------------------------------------------------------------------</p>
<h3>Results_and_Display_Settings</h3>
<p>-------------------------------------------------------------------------------------------------------------</p>
<h3>order</h3>
<p></p>

<h3>get_score_average</h3>
<p></p>

<h3>get_score_entropy</h3>
<p></p>

<h3>spaces_in_output</h3>
<p></p>

<h3>display_iterations</h3>
<p></p>

<h3>display_num_different</h3>
<p></p>

<h3>display_score_average</h3>
<p></p>

<h3>dispaly_score_entropy</h3>
<p></p>

<p>-------------------------------------------------------------------------------------------------------------</p>
<h3>Logging</h3>
<p>-------------------------------------------------------------------------------------------------------------</p>

<h3>write_log_file</h3>
<p></p>

<h3>number_to_log</h3>
<p></p>
</body>