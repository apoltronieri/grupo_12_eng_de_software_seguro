<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>ZAP by Checkmarx Scanning Report</title>
<link
	href="relatorio-zap/normalize/normalize.css" rel="stylesheet">
<link
	href="relatorio-zap/themes/original/main.css" rel="stylesheet">
<link
	href="relatorio-zap/themes/original/colors.css" rel="stylesheet">
</head>
<body>
	<header>
		<h1>ZAP by Checkmarx Scanning Report</h1>
		<p>
			<span>Generated with</span> <a href="https://zaproxy.org"><img
				src="relatorio-zap/zap32x32.png" alt="The ZAP logo" class="zap-logo">ZAP</a>
			<span>on qua. 12 ago. 2026, at 16:56:57</span>
		</p>
		<p>ZAP Version: 2.17.0</p>
		<p>
			ZAP by <a href="https://checkmarx.com/">Checkmarx</a>
		</p>
	</header>

	<main>

		<section id="contents" class="contents">
			<h2>Contents</h2>
			<nav>
				<ol>
					<li><a
						href="#about-this-report">About This Report</a>
						<ol>
							
							<li><a
								href="#report-parameters">Report Parameters</a></li>
						</ol></li>
					<data-th-block>
					<li><a
						href="#summaries">Summaries</a>
						<ol>
							<li><a
								href="#risk-confidence-counts">Alert Counts by Risk and Confidence</a></li>
							<li><a
								href="#site-risk-counts">Alert Counts by Site and Risk</a></li>
							<li><a
								href="#alert-type-counts">Alert Counts by Alert Type</a></li>
							<li><a
								href="#insights">Insights</a></li>
						</ol></li>
					<li><a
						href="#alerts">Alerts</a>
						<ol>
							
							 
							
							
							
							
							<li><a
								href="#alerts--risk-2-confidence-3"><span>Risk</span>=<span
									class="risk-level">Médio</span>, <span>Confidence</span>=<span
									class="confidence-level">Alto</span> <span>(1)</span></a></li>
							
							<li><a
								href="#alerts--risk-2-confidence-2"><span>Risk</span>=<span
									class="risk-level">Médio</span>, <span>Confidence</span>=<span
									class="confidence-level">Médio</span> <span>(1)</span></a></li>
							
							
							  
							
							
							
							
							<li><a
								href="#alerts--risk-1-confidence-3"><span>Risk</span>=<span
									class="risk-level">Baixo</span>, <span>Confidence</span>=<span
									class="confidence-level">Alto</span> <span>(1)</span></a></li>
							
							
							
							<li><a
								href="#alerts--risk-1-confidence-1"><span>Risk</span>=<span
									class="risk-level">Baixo</span>, <span>Confidence</span>=<span
									class="confidence-level">Baixo</span> <span>(1)</span></a></li>
							  
							
							
							
							
							
							
							<li><a
								href="#alerts--risk-0-confidence-2"><span>Risk</span>=<span
									class="risk-level">Informativo</span>, <span>Confidence</span>=<span
									class="confidence-level">Médio</span> <span>(1)</span></a></li>
							
							<li><a
								href="#alerts--risk-0-confidence-1"><span>Risk</span>=<span
									class="risk-level">Informativo</span>, <span>Confidence</span>=<span
									class="confidence-level">Baixo</span> <span>(1)</span></a></li>
							  
						</ol></li>
					<li><a
						href="#appendix">Appendix</a>
						<ol>
							<li><a
								href="#alert-types">Alert Types</a></li>
						</ol></li>
					</data-th-block>
				</ol>
			</nav>
		</section>

		<section
			id="about-this-report" class="about-this-report">
			<h2>About This Report</h2>

			

			<section
				id="report-parameters">
				<h3>Report Parameters</h3>
				<div class="report-parameters--container">
					<h4>Contexts</h4>
					
					
					<p>No contexts were selected, so all contexts were included by default.</p>
					  

					<h4>Sites</h4>
					
					<p>The following sites were included:</p>
					<ul class="sites-list">
						<li><span class="site">https://juice-shop.herokuapp.com</span></li>
					</ul>
					
					<p>(If no sites were selected, all sites were included by default.)</p>
					<p>An included site must also be within one of the included contexts for its data to be included in the report.</p>

					<h4>Risk levels</h4>
					<p>
						<span>Included</span>:
						 
						<span class="included-risk-codes"><span class="risk-level">Alto</span>, <span class="risk-level">Médio</span>, <span class="risk-level">Baixo</span>, <span class="risk-level">Informativo</span></span>
					</p>
					<p>
						<span>Excluded</span>:
						 <span>None</span>
						
					</p>

					<h4>Confidence levels</h4>
					<p>
						<span>Included</span>:
						
						
						<span class="included-confidence-codes"><span class="confidence-level">User Confirmed</span>, <span class="confidence-level">Alto</span>, <span class="confidence-level">Médio</span>, <span class="confidence-level">Baixo</span></span>
					</p>
					<p>
						<span>Excluded</span>:
						
						
						<span class="included-confidence-codes"> <span class="confidence-level">User Confirmed</span>, <span class="confidence-level">Alto</span>, <span class="confidence-level">Médio</span>, <span class="confidence-level">Baixo</span>, <span class="confidence-level">Falso Positivo</span></span>
					</p>
				</div>
			</section>
		</section>

		
		<section>
			
		</section>
		
		<section id="summaries" class="summaries">
			<h2>Summaries</h2>

			<section
				id="risk-confidence-counts">
				<h3>Alert Counts by Risk and Confidence</h3>
				<table class="risk-confidence-counts-table">
					<caption>
						<p>This table shows the number of alerts for each level of risk and confidence included in the report.</p>
						<p>(The percentages in brackets represent the count as a percentage of the total number of alerts included in the report, rounded to one decimal place.)</p>
					</caption>
					<colgroup>
						<col>
						<col>
					</colgroup>
					<colgroup>
						<col
							style="width: 14.0%"><col
							style="width: 14.0%"><col
							style="width: 14.0%"><col
							style="width: 14.0%">
						<col style="width: 14.0%">
					</colgroup>
					<thead>
						<tr>
							<td colspan="2" rowspan="2"></td>
							<th scope="colgroup"
								colspan="5">Confidence</th>
						</tr>
						<tr>
							<th scope="col">User Confirmed</th>
							<th scope="col">Alto</th>
							<th scope="col">Médio</th>
							<th scope="col">Baixo</th>
							<th scope="col">Total</th>
						</tr>
					</thead>
					<tbody>
						<tr>
							<th scope="rowgroup"
								rowspan="5">Risk</th>
							<th scope="row">Alto</th>
							<td><span>0</span><br> <span
								class="additional-info-percentages">(0,0%)</span></td>
							<td><span>0</span><br> <span
								class="additional-info-percentages">(0,0%)</span></td>
							<td><span>0</span><br> <span
								class="additional-info-percentages">(0,0%)</span></td>
							<td><span>0</span><br> <span
								class="additional-info-percentages">(0,0%)</span></td>
							<td><span>0</span><br> <span class="additional-info-percentages">(0,0%)</span></td>
						</tr>
						<tr>
							
							<th scope="row">Médio</th>
							<td><span>0</span><br> <span
								class="additional-info-percentages">(0,0%)</span></td>
							<td><span>1</span><br> <span
								class="additional-info-percentages">(16,7%)</span></td>
							<td><span>1</span><br> <span
								class="additional-info-percentages">(16,7%)</span></td>
							<td><span>0</span><br> <span
								class="additional-info-percentages">(0,0%)</span></td>
							<td><span>2</span><br> <span class="additional-info-percentages">(33,3%)</span></td>
						</tr>
						<tr>
							
							<th scope="row">Baixo</th>
							<td><span>0</span><br> <span
								class="additional-info-percentages">(0,0%)</span></td>
							<td><span>1</span><br> <span
								class="additional-info-percentages">(16,7%)</span></td>
							<td><span>0</span><br> <span
								class="additional-info-percentages">(0,0%)</span></td>
							<td><span>1</span><br> <span
								class="additional-info-percentages">(16,7%)</span></td>
							<td><span>2</span><br> <span class="additional-info-percentages">(33,3%)</span></td>
						</tr>
						<tr>
							
							<th scope="row">Informativo</th>
							<td><span>0</span><br> <span
								class="additional-info-percentages">(0,0%)</span></td>
							<td><span>0</span><br> <span
								class="additional-info-percentages">(0,0%)</span></td>
							<td><span>1</span><br> <span
								class="additional-info-percentages">(16,7%)</span></td>
							<td><span>1</span><br> <span
								class="additional-info-percentages">(16,7%)</span></td>
							<td><span>2</span><br> <span class="additional-info-percentages">(33,3%)</span></td>
						</tr>
						<tr>
							<th scope="row">Total</th>
							<td><span>0</span><br> <span
								class="additional-info-percentages">(0,0%)</span></td>
							<td><span>2</span><br> <span
								class="additional-info-percentages">(33,3%)</span></td>
							<td><span>2</span><br> <span
								class="additional-info-percentages">(33,3%)</span></td>
							<td><span>2</span><br> <span
								class="additional-info-percentages">(33,3%)</span></td>
							<td><span>6</span><br> <span
								class="additional-info-percentages">(100%)</span></td>
						</tr>
					</tbody>
				</table>
			</section>

			<section
				id="site-risk-counts">
				<h3>Alert Counts by Site and Risk</h3>
				<table class="site-risk-counts-table">
					<caption>
						<p>This table shows, for each site for which one or more alerts were raised, the number of alerts raised at each risk level.</p>
						<p>Alerts with a confidence level of &quot;False Positive&quot; have been excluded from these counts.</p>
						<p>(The numbers in brackets are the number of alerts raised for the site at or above that risk level.)</p>
					</caption>
					<colgroup>
						<col>
						<col>
					</colgroup>
					<colgroup>
						<col
							style="width: 16.25%"><col
							style="width: 16.25%"><col
							style="width: 16.25%"><col
							style="width: 16.25%">
					</colgroup>
					<thead>
						<tr>
							<td colspan="2" rowspan="2"></td>
							<th scope="colgroup" colspan="4">Risk</th>
						</tr>
						<tr>
							<th scope="col">
								<span>Alto</span><br>  <span
									class="additional-info-percentages">(= Alto)</span>  
							</th>
							<th scope="col">
								<span>Médio</span><br>   <span
									class="additional-info-percentages">(&gt;= Médio)</span> 
							</th>
							<th scope="col">
								<span>Baixo</span><br>   <span
									class="additional-info-percentages">(&gt;= Baixo)</span> 
							</th>
							<th scope="col">
								<span>Informativo</span><br>   <span
									class="additional-info-percentages">(&gt;= Informativo)</span> 
							</th>
						</tr>
					</thead>
					<tbody>
						<tr>
							<th scope="rowgroup"
								rowspan="1">Site</th>
							<th scope="row">https://juice-shop.herokuapp.com</th>
							
							<td><span>0</span><br> <span
								class="additional-info-percentages">(0)</span></td>
							<td><span>2</span><br> <span
								class="additional-info-percentages">(2)</span></td>
							<td><span>2</span><br> <span
								class="additional-info-percentages">(4)</span></td>
							<td><span>2</span><br> <span
								class="additional-info-percentages">(6)</span></td>
							
						</tr>
					</tbody>
				</table>
			</section>

			<section
				id="alert-type-counts">
				<h3>Alert Counts by Alert Type</h3>
				<table class="alert-type-counts-table">
					<caption>
						<p>This table shows the number of alerts of each alert type, together with the alert type&#39;s risk level.</p>
						<p>(The percentages in brackets represent each count as a percentage, rounded to one decimal place, of the total number of alerts included in this report.)</p>
					</caption>
					<thead>
						<tr>
							<th scope="col">Alert type</th>
							<th scope="col">Risk</th>
							<th scope="col">Count</th>
						</tr>
					</thead>
					<tbody>
						<tr>
							<th scope="row"><a
								href="#alert-type-0">Configuração Incorreta Entre Domínios</a></th>
							<td class="risk-level">Médio</td>
							<td><span>5</span><br> <span
								class="additional-info-percentages">(83,3%)</span></td>
						</tr>
						<tr>
							<th scope="row"><a
								href="#alert-type-1">Content Security Policy (CSP) Header Not Set</a></th>
							<td class="risk-level">Médio</td>
							<td><span>5</span><br> <span
								class="additional-info-percentages">(83,3%)</span></td>
						</tr>
						<tr>
							<th scope="row"><a
								href="#alert-type-2">Divulgação de Data e Hora - Unix</a></th>
							<td class="risk-level">Baixo</td>
							<td><span>5</span><br> <span
								class="additional-info-percentages">(83,3%)</span></td>
						</tr>
						<tr>
							<th scope="row"><a
								href="#alert-type-3">Strict-Transport-Security Header Not Set</a></th>
							<td class="risk-level">Baixo</td>
							<td><span>5</span><br> <span
								class="additional-info-percentages">(83,3%)</span></td>
						</tr>
						<tr>
							<th scope="row"><a
								href="#alert-type-4">Modern Web Application</a></th>
							<td class="risk-level">Informativo</td>
							<td><span>5</span><br> <span
								class="additional-info-percentages">(83,3%)</span></td>
						</tr>
						<tr>
							<th scope="row"><a
								href="#alert-type-5">Re-examine Cache-control Directives</a></th>
							<td class="risk-level">Informativo</td>
							<td><span>5</span><br> <span
								class="additional-info-percentages">(83,3%)</span></td>
						</tr>
					</tbody>
					<tfoot>
						<tr>
							<th scope="row">Total</th>
							<td></td>
							<td>6</td>
						</tr>
					</tfoot>
				</table>
			</section>
			
			<section
				id="insights">
				<h3 class="left-header">Insights</h3>
				
				<table class="insights-table">
						<caption>
						<p>This table shows information that is likely to be very relevant to you, but which is not related to vulnerabilities, or potentially even related to the application in question.</p>
					</caption>
					<thead>
					<tr>
						<th scope="col">Level</th>
						<th scope="col">Motivo</th>
						<th scope="col">Site</th>
						<th scope="col">Descrição</th>
						<th scope="col">Statistic</th>
					</tr>
					</thead>
					<tbody>
					
						<tr>
						<td class="risk-2">
							<div>Médio</div>
						</td>
						<td>
							<div>Exceeded Low</div>
						</td>
						<td>
							<div></div>
						</td>
						<td>
							<div>Percentage of memory used</div>
						</td>
						<td align="center">
							<div>80   </div>
						</td>
						</tr>
					
						<tr>
						<td class="risk-1">
							<div>Baixo</div>
						</td>
						<td>
							<div>Atenção</div>
						</td>
						<td>
							<div></div>
						</td>
						<td>
							<div>ZAP errors logged - see the zap.log file for details</div>
						</td>
						<td align="center">
							<div>3   </div>
						</td>
						</tr>
					
						<tr>
						<td class="risk-1">
							<div>Baixo</div>
						</td>
						<td>
							<div>Atenção</div>
						</td>
						<td>
							<div></div>
						</td>
						<td>
							<div>ZAP warnings logged - see the zap.log file for details</div>
						</td>
						<td align="center">
							<div>8   </div>
						</td>
						</tr>
					
						<tr>
						<td class="risk-0">
							<div>Informações</div>
						</td>
						<td>
							<div>Informativo</div>
						</td>
						<td>
							<div>https://juice-shop.herokuapp.com</div>
						</td>
						<td>
							<div>Percentage of responses with status code 2xx</div>
						</td>
						<td align="center">
							<div>26 %</div>
						</td>
						</tr>
					
						<tr>
						<td class="risk-0">
							<div>Informações</div>
						</td>
						<td>
							<div>Informativo</div>
						</td>
						<td>
							<div>https://juice-shop.herokuapp.com</div>
						</td>
						<td>
							<div>Percentage of responses with status code 4xx</div>
						</td>
						<td align="center">
							<div>1 %</div>
						</td>
						</tr>
					
						<tr>
						<td class="risk-0">
							<div>Informações</div>
						</td>
						<td>
							<div>Informativo</div>
						</td>
						<td>
							<div>https://juice-shop.herokuapp.com</div>
						</td>
						<td>
							<div>Percentage of responses with status code 5xx</div>
						</td>
						<td align="center">
							<div>70 %</div>
						</td>
						</tr>
					
						<tr>
						<td class="risk-0">
							<div>Informações</div>
						</td>
						<td>
							<div>Informativo</div>
						</td>
						<td>
							<div>https://juice-shop.herokuapp.com</div>
						</td>
						<td>
							<div>Percentage of endpoints with content type application/javascript</div>
						</td>
						<td align="center">
							<div>4 %</div>
						</td>
						</tr>
					
						<tr>
						<td class="risk-0">
							<div>Informações</div>
						</td>
						<td>
							<div>Informativo</div>
						</td>
						<td>
							<div>https://juice-shop.herokuapp.com</div>
						</td>
						<td>
							<div>Percentage of endpoints with content type application/octet-stream</div>
						</td>
						<td align="center">
							<div>7 %</div>
						</td>
						</tr>
					
						<tr>
						<td class="risk-0">
							<div>Informações</div>
						</td>
						<td>
							<div>Informativo</div>
						</td>
						<td>
							<div>https://juice-shop.herokuapp.com</div>
						</td>
						<td>
							<div>Percentage of endpoints with content type image/x-icon</div>
						</td>
						<td align="center">
							<div>1 %</div>
						</td>
						</tr>
					
						<tr>
						<td class="risk-0">
							<div>Informações</div>
						</td>
						<td>
							<div>Informativo</div>
						</td>
						<td>
							<div>https://juice-shop.herokuapp.com</div>
						</td>
						<td>
							<div>Percentage of endpoints with content type text/css</div>
						</td>
						<td align="center">
							<div>1 %</div>
						</td>
						</tr>
					
						<tr>
						<td class="risk-0">
							<div>Informações</div>
						</td>
						<td>
							<div>Informativo</div>
						</td>
						<td>
							<div>https://juice-shop.herokuapp.com</div>
						</td>
						<td>
							<div>Percentage of endpoints with content type text/html</div>
						</td>
						<td align="center">
							<div>78 %</div>
						</td>
						</tr>
					
						<tr>
						<td class="risk-0">
							<div>Informações</div>
						</td>
						<td>
							<div>Informativo</div>
						</td>
						<td>
							<div>https://juice-shop.herokuapp.com</div>
						</td>
						<td>
							<div>Percentage of endpoints with content type text/markdown</div>
						</td>
						<td align="center">
							<div>4 %</div>
						</td>
						</tr>
					
						<tr>
						<td class="risk-0">
							<div>Informações</div>
						</td>
						<td>
							<div>Informativo</div>
						</td>
						<td>
							<div>https://juice-shop.herokuapp.com</div>
						</td>
						<td>
							<div>Percentage of endpoints with content type text/plain</div>
						</td>
						<td align="center">
							<div>1 %</div>
						</td>
						</tr>
					
						<tr>
						<td class="risk-0">
							<div>Informações</div>
						</td>
						<td>
							<div>Informativo</div>
						</td>
						<td>
							<div>https://juice-shop.herokuapp.com</div>
						</td>
						<td>
							<div>Percentage of endpoints with method GET</div>
						</td>
						<td align="center">
							<div>100 %</div>
						</td>
						</tr>
					
						<tr>
						<td class="risk-0">
							<div>Informações</div>
						</td>
						<td>
							<div>Informativo</div>
						</td>
						<td>
							<div>https://juice-shop.herokuapp.com</div>
						</td>
						<td>
							<div>Count of total endpoints</div>
						</td>
						<td align="center">
							<div>65   </div>
						</td>
						</tr>
					
						<tr>
						<td class="risk-0">
							<div>Informações</div>
						</td>
						<td>
							<div>Informativo</div>
						</td>
						<td>
							<div>https://juice-shop.herokuapp.com</div>
						</td>
						<td>
							<div>Percentage of slow responses</div>
						</td>
						<td align="center">
							<div>100 %</div>
						</td>
						</tr>
					
					</tbody>
				</table>
			</section>
		</section>

		<section id="alerts" class="alerts">
			<h2>Alerts</h2>
			<ol>
				
				 
				 
				
				
				<li id="alerts--risk-2-confidence-3">
					<h3>
						<span>Risk</span>=<span
							class="risk-level">Médio</span>, <span>Confidence</span>=<span
							class="confidence-level">Alto</span> <span>(1)</span>
					</h3>
					<ol>
						
						<li class="alerts--site-li">
							<h4>
								<span class="site">https://juice-shop.herokuapp.com</span> <span>(1)</span>
							</h4>
							<ol>
								
								<li>
									<h5>
										<a
											href="#alert-type-1">Content Security Policy (CSP) Header Not Set</a> <span>(1)</span>
									</h5>
									<ol>
										<li><details>
												<summary>
													<span class="request-method-n-url">GET https://juice-shop.herokuapp.com</span>
												</summary>
												
<table class="alerts-table">
	<tr>
		<th scope="row">Alert tags</th>
		<td>
			<ul class="alert-tags-list">
				<li>
					<span><a href="https://owasp.org/Top10/A05_2021-Security_Misconfiguration/">OWASP_2021_A05</a></span> 
				</li>
				<li>
					 <span>POLICY_QA_STD = </span>
				</li>
				<li>
					 <span>POLICY_PENTEST = </span>
				</li>
				<li>
					<span><a href="https://www.zaproxy.org/docs/desktop/addons/common-library/alerttags/#systemic">SYSTEMIC</a></span> 
				</li>
				<li>
					<span><a href="https://cwe.mitre.org/data/definitions/693.html">CWE-693</a></span> 
				</li>
				<li>
					<span><a href="https://owasp.org/www-project-top-ten/2017/A6_2017-Security_Misconfiguration.html">OWASP_2017_A06</a></span> 
				</li>
				<li>
					<span><a href="https://owasp.org/Top10/2025/A02_2025-Security_Misconfiguration/">OWASP_2025_A02</a></span> 
				</li>
			</ul>
		</td>
	</tr>
	<tr>
		<th scope="row">Alert description</th>
		<td> 
<p>Content Security Policy (CSP) is an added layer of security that helps to detect and mitigate certain types of attacks, including Cross Site Scripting (XSS) and data injection attacks. These attacks are used for everything from data theft to site defacement or distribution of malware. CSP provides a set of standard HTTP headers that allow website owners to declare approved sources of content that browsers should be allowed to load on that page — covered types are JavaScript, CSS, HTML frames, fonts, images and embeddable objects such as Java applets, ActiveX, audio and video files.</p>
 </td>
	</tr>
	
	<tr>
		<th scope="row">Request</th>
		<td><details open="open">
				<summary>Request line and header section (249 bytes)</summary>
				
				<pre><code>GET https://juice-shop.herokuapp.com HTTP/1.1
host: juice-shop.herokuapp.com
user-agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/141.0.0.0 Safari/537.36
pragma: no-cache
cache-control: no-cache

</code></pre>
				
				
			</details> <details class="request-body" open="open">
				<summary>Request body (0 bytes)</summary>
				
				<pre><code></code></pre>
				
				
			</details></td>
	</tr>
	<tr>
		<th scope="row">Response</th>
		<td><details open="open">
				<summary>Status line and header section (979 bytes)</summary>
				
				<pre><code>HTTP/1.1 200 OK
Accept-Ranges: bytes
Access-Control-Allow-Origin: *
Cache-Control: public, max-age=0
Content-Length: 9393
Content-Type: text/html; charset=UTF-8
Date: Wed, 12 Aug 2026 19:28:49 GMT
Etag: W/&quot;24b1-19ff772555c&quot;
Feature-Policy: payment &#39;self&#39;
Last-Modified: Wed, 12 Aug 2026 19:28:13 GMT
Nel: {&quot;report_to&quot;:&quot;heroku-nel&quot;,&quot;response_headers&quot;:[&quot;Via&quot;],&quot;max_age&quot;:3600,&quot;success_fraction&quot;:0.01,&quot;failure_fraction&quot;:0.1}
Report-To: {&quot;group&quot;:&quot;heroku-nel&quot;,&quot;endpoints&quot;:[{&quot;url&quot;:&quot;https://nel.heroku.com/reports?s=XJ%2BSWZNyTjZrXjtc88LQgL%2FIIBv5MD1mBAlUm4hYMKE%3D\u0026sid=812dcc77-0bd0-43b1-a5f1-b25750382959\u0026ts=1786562929&quot;}],&quot;max_age&quot;:3600}
Reporting-Endpoints: heroku-nel=&quot;https://nel.heroku.com/reports?s=XJ%2BSWZNyTjZrXjtc88LQgL%2FIIBv5MD1mBAlUm4hYMKE%3D&amp;sid=812dcc77-0bd0-43b1-a5f1-b25750382959&amp;ts=1786562929&quot;
Server: Heroku
Vary: Accept-Encoding
Via: 1.1 heroku-router
X-Content-Type-Options: nosniff
X-Frame-Options: SAMEORIGIN
X-Recruiting: /#/jobs

</code></pre>
				
				
			</details> <details class="response-body">
				<summary>Response body (9393 bytes)</summary>
				
				<pre><code>&lt;!--
  ~ Copyright (c) 2014-2026 Bjoern Kimminich &amp; the OWASP Juice Shop contributors.
  ~ SPDX-License-Identifier: MIT
  --&gt;

&lt;!doctype html&gt;
&lt;html lang=&quot;en&quot; data-beasties-container&gt;
&lt;head&gt;
  &lt;meta charset=&quot;utf-8&quot;&gt;
  &lt;title&gt;OWASP Juice Shop&lt;/title&gt;
  &lt;meta name=&quot;description&quot; content=&quot;Probably the most modern and sophisticated insecure web application&quot;&gt;
  &lt;meta name=&quot;viewport&quot; content=&quot;width=device-width, initial-scale=1&quot;&gt;
  &lt;link rel=&quot;preconnect&quot; href=&quot;https://fonts.googleapis.com&quot;&gt;
  &lt;link rel=&quot;preconnect&quot; href=&quot;https://fonts.gstatic.com&quot; crossorigin&gt;
  &lt;style&gt;@font-face{font-family:&#39;VT323&#39;;font-style:normal;font-weight:400;font-display:swap;src:url(https://fonts.gstatic.com/s/vt323/v18/pxiKyp0ihIEF2isQFJXGdg.woff2) format(&#39;woff2&#39;);unicode-range:U+0102-0103, U+0110-0111, U+0128-0129, U+0168-0169, U+01A0-01A1, U+01AF-01B0, U+0300-0301, U+0303-0304, U+0308-0309, U+0323, U+0329, U+1EA0-1EF9, U+20AB;}@font-face{font-family:&#39;VT323&#39;;font-style:normal;font-weight:400;font-display:swap;src:url(https://fonts.gstatic.com/s/vt323/v18/pxiKyp0ihIEF2isRFJXGdg.woff2) format(&#39;woff2&#39;);unicode-range:U+0100-02BA, U+02BD-02C5, U+02C7-02CC, U+02CE-02D7, U+02DD-02FF, U+0304, U+0308, U+0329, U+1D00-1DBF, U+1E00-1E9F, U+1EF2-1EFF, U+2020, U+20A0-20AB, U+20AD-20C0, U+2113, U+2C60-2C7F, U+A720-A7FF;}@font-face{font-family:&#39;VT323&#39;;font-style:normal;font-weight:400;font-display:swap;src:url(https://fonts.gstatic.com/s/vt323/v18/pxiKyp0ihIEF2isfFJU.woff2) format(&#39;woff2&#39;);unicode-range:U+0000-00FF, U+0131, U+0152-0153, U+02BB-02BC, U+02C6, U+02DA, U+02DC, U+0304, U+0308, U+0329, U+2000-206F, U+20AC, U+2122, U+2191, U+2193, U+2212, U+2215, U+FEFF, U+FFFD;}&lt;/style&gt;
  &lt;link id=&quot;favicon&quot; rel=&quot;icon&quot; type=&quot;image/x-icon&quot; href=&quot;assets/public/favicon_js.ico&quot;&gt;
  &lt;script&gt;
    window.addEventListener(&quot;load&quot;, function(){
      window.cookieconsent.initialise({
        &quot;palette&quot;: {
          &quot;popup&quot;: { &quot;background&quot;: &quot;var(--theme-primary)&quot;, &quot;text&quot;: &quot;var(--theme-text)&quot; },
          &quot;button&quot;: { &quot;background&quot;: &quot;var(--theme-accent)&quot;, &quot;text&quot;: &quot;var(--theme-text)&quot; }
        },
        &quot;theme&quot;: &quot;classic&quot;,
        &quot;position&quot;: &quot;bottom-right&quot;,
        &quot;content&quot;: { &quot;message&quot;: &quot;This website uses fruit cookies to ensure you get the juiciest tracking experience.&quot;, &quot;dismiss&quot;: &quot;Me want it!&quot;, &quot;link&quot;: &quot;But me wait!&quot;, &quot;href&quot;: &quot;https://www.youtube.com/watch?v=9PnbKL3wuH4&quot; }
      })});
  &lt;/script&gt;
&lt;style&gt;.bluegrey-lightgreen-theme{--mat-sys-background:#121316;--mat-sys-error:#ffb4ab;--mat-sys-error-container:#93000a;--mat-sys-inverse-on-surface:#2f3033;--mat-sys-inverse-primary:#005cbb;--mat-sys-inverse-surface:#e3e2e6;--mat-sys-on-background:#e3e2e6;--mat-sys-on-error:#690005;--mat-sys-on-error-container:#ffdad6;--mat-sys-on-primary:#002f65;--mat-sys-on-primary-container:#d7e3ff;--mat-sys-on-primary-fixed:#001b3f;--mat-sys-on-primary-fixed-variant:#00458f;--mat-sys-on-secondary:#283041;--mat-sys-on-secondary-container:#dae2f9;--mat-sys-on-secondary-fixed:#131c2b;--mat-sys-on-secondary-fixed-variant:#3e4759;--mat-sys-on-surface:#e3e2e6;--mat-sys-on-surface-variant:#e0e2ec;--mat-sys-on-tertiary:#173800;--mat-sys-on-tertiary-container:#82ff10;--mat-sys-on-tertiary-fixed:#0b2000;--mat-sys-on-tertiary-fixed-variant:#245100;--mat-sys-outline:#8e9099;--mat-sys-outline-variant:#44474e;--mat-sys-primary:#abc7ff;--mat-sys-primary-container:#00458f;--mat-sys-primary-fixed:#d7e3ff;--mat-sys-primary-fixed-dim:#abc7ff;--mat-sys-scrim:#000000;--mat-sys-secondary:#bec6dc;--mat-sys-secondary-container:#3e4759;--mat-sys-secondary-fixed:#dae2f9;--mat-sys-secondary-fixed-dim:#bec6dc;--mat-sys-shadow:#000000;--mat-sys-surface:#121316;--mat-sys-surface-bright:#38393c;--mat-sys-surface-container:#1f2022;--mat-sys-surface-container-high:#292a2c;--mat-sys-surface-container-highest:#343537;--mat-sys-surface-container-low:#1a1b1f;--mat-sys-surface-container-lowest:#0d0e11;--mat-sys-surface-dim:#121316;--mat-sys-surface-tint:#abc7ff;--mat-sys-surface-variant:#44474e;--mat-sys-tertiary:#70e000;--mat-sys-tertiary-container:#245100;--mat-sys-tertiary-fixed:#82ff10;--mat-sys-tertiary-fixed-dim:#70e000;--mat-sys-neutral-variant20:#2d3038;--mat-sys-neutral10:#1a1b1f;--mat-sys-level0:0px 0px 0px 0px rgba(0, 0, 0, .2), 0px 0px 0px 0px rgba(0, 0, 0, .14), 0px 0px 0px 0px rgba(0, 0, 0, .12);--mat-sys-level1:0px 2px 1px -1px rgba(0, 0, 0, .2), 0px 1px 1px 0px rgba(0, 0, 0, .14), 0px 1px 3px 0px rgba(0, 0, 0, .12);--mat-sys-level2:0px 3px 3px -2px rgba(0, 0, 0, .2), 0px 3px 4px 0px rgba(0, 0, 0, .14), 0px 1px 8px 0px rgba(0, 0, 0, .12);--mat-sys-level3:0px 3px 5px -1px rgba(0, 0, 0, .2), 0px 6px 10px 0px rgba(0, 0, 0, .14), 0px 1px 18px 0px rgba(0, 0, 0, .12);--mat-sys-level4:0px 5px 5px -3px rgba(0, 0, 0, .2), 0px 8px 10px 1px rgba(0, 0, 0, .14), 0px 3px 14px 2px rgba(0, 0, 0, .12);--mat-sys-level5:0px 7px 8px -4px rgba(0, 0, 0, .2), 0px 12px 17px 2px rgba(0, 0, 0, .14), 0px 5px 22px 4px rgba(0, 0, 0, .12);--mat-sys-corner-extra-large:28px;--mat-sys-corner-extra-large-top:28px 28px 0 0;--mat-sys-corner-extra-small:4px;--mat-sys-corner-extra-small-top:4px 4px 0 0;--mat-sys-corner-full:9999px;--mat-sys-corner-large:16px;--mat-sys-corner-large-end:0 16px 16px 0;--mat-sys-corner-large-start:16px 0 0 16px;--mat-sys-corner-large-top:16px 16px 0 0;--mat-sys-corner-medium:12px;--mat-sys-corner-none:0;--mat-sys-corner-small:8px;--mat-sys-dragged-state-layer-opacity:.16;--mat-sys-focus-state-layer-opacity:.12;--mat-sys-hover-state-layer-opacity:.08;--mat-sys-pressed-state-layer-opacity:.12;color:var(--mat-sys-on-surface);background-color:var(--mat-sys-surface)}html{font-family:var(--mat-sys-body-medium-font, Roboto, &quot;Helvetica Neue&quot;, sans-serif)}.bluegrey-lightgreen-theme{--theme-primary:#438fff;--theme-primary-lighter:rgb(97.6, 161.229787234, 255);--theme-primary-light:rgb(118, 173.3829787234, 255);--theme-primary-darker:rgb(36.4, 124.770212766, 255);--theme-primary-dark:rgb(16, 112.6170212766, 255);--theme-primary-fade-10:#438fff;--theme-primary-fade-20:#438fff;--theme-primary-fade-30:#438fff;--theme-primary-fade-40:#438fff;--theme-primary-fade-50:#438fff;--theme-accent:#50a400;--theme-accent-lighter:rgb(94.9268292683, 194.6, 0);--theme-accent-light:rgb(104.8780487805, 215, 0);--theme-accent-darker:rgb(65.0731707317, 133.4, 0);--theme-accent-dark:rgb(55.1219512195, 113, 0);--theme-accent-fade-10:#50a400;--theme-accent-fade-20:#50a400;--theme-accent-fade-30:#50a400;--theme-accent-fade-40:#50a400;--theme-accent-fade-50:#50a400;--theme-warn:#ffb4ab;--theme-warn-lighter:rgb(255, 207.3214285714, 201.6);--theme-warn-light:rgb(255, 225.5357142857, 222);--theme-warn-darker:rgb(255, 152.6785714286, 140.4);--theme-warn-dark:rgb(255, 134.4642857143, 120);--theme-warn-fade-10:#ffb4ab;--theme-warn-fade-20:#ffb4ab;--theme-warn-fade-30:#ffb4ab;--theme-warn-fade-40:#ffb4ab;--theme-warn-fade-50:#ffb4ab;--theme-text:#e3e2e6;--theme-text-lighter:rgb(242.8666666667, 242.4333333333, 244.1666666667);--theme-text-light:rgb(253.4444444444, 253.3888888889, 253.6111111111);--theme-text-darker:rgb(200.5555555556, 198.6111111111, 206.3888888889);--theme-text-dark:rgb(160.8888888889, 157.5277777778, 170.9722222222);--theme-text-fade-10:#e3e2e6;--theme-text-fade-20:#e3e2e6;--theme-text-fade-30:#e3e2e6;--theme-text-fade-40:#e3e2e6;--theme-text-fade-50:#e3e2e6;--theme-text-invert-15:rgb(197.15, 196.45, 199.25);--theme-text-invert-30:rgb(167.3, 166.9, 168.5);--theme-background:#1f2022;--theme-background-lighter:rgb(45.5938461538, 47.0646153846, 50.0061538462);--theme-background-light:rgb(55.3230769231, 57.1076923077, 60.6769230769);--theme-background-darker:rgb(16.4061538462, 16.9353846154, 17.9938461538);--theme-background-dark:rgb(6.6769230769, 6.8923076923, 7.3230769231);--theme-background-darkest:hsl(220, 4.6153846154%, -1.2549019608%);--theme-thumbnail-border:1px solid #abc7ff;--mdc-filled-text-field-container-color:#0000;--mdc-filled-text-field-disabled-container-color:#0000;--theme-background:#3e3e3e;--theme-background-lighter:#4a4a4a;--theme-background-light:#5a5a5a;--theme-background-darker:#333638;--theme-background-dark:#303030;--theme-background-darkest:#2b2b2b;--theme-text:#e8ecef;--theme-text-lighter:#f2f5f7;--theme-text-light:#fff;--theme-text-darker:#b8c0c7;--theme-text-dark:#7f8a93;--mat-sys-surface:#333638;--mat-sys-on-surface:#e8ecef;--mat-sys-surface-container:#3e3e3e;--mat-sys-surface-container-high:#404244;--mat-sys-on-surface-variant:#b8c0c7;--mat-sys-outline:#5a5a5a;--mat-sys-outline-variant:#404244}.bluegrey-lightgreen-theme{--theme-warn:#f44336;--theme-warn-lighter:rgb(245.5877358491, 94.1358490566, 83.0122641509);--theme-warn-light:rgb(246.6462264151, 112.2264150943, 102.3537735849);--theme-warn-darker:rgb(242.4122641509, 39.8641509434, 24.9877358491);--theme-warn-dark:rgb(234.1839622642, 27.9622641509, 12.8160377358);--theme-warn-fade-10:#f44336;--theme-warn-fade-20:#f44336;--theme-warn-fade-30:#f44336;--theme-warn-fade-40:#f44336;--theme-warn-fade-50:#f44336;--mat-sys-error:#f44336;--mat-sys-on-error:#fff}@media screen and (-webkit-min-device-pixel-ratio:0){}&lt;/style&gt;&lt;link rel=&quot;stylesheet&quot; href=&quot;styles.css&quot; media=&quot;print&quot; onload=&quot;this.media=&#39;all&#39;&quot;&gt;&lt;noscript&gt;&lt;link rel=&quot;stylesheet&quot; href=&quot;styles.css&quot;&gt;&lt;/noscript&gt;&lt;/head&gt;
&lt;body class=&quot;bluegrey-lightgreen-theme&quot;&gt;
  &lt;app-root&gt;&lt;/app-root&gt;
&lt;script src=&quot;polyfills.js&quot; type=&quot;module&quot;&gt;&lt;/script&gt;&lt;script src=&quot;scripts.js&quot; defer&gt;&lt;/script&gt;&lt;script src=&quot;main.js&quot; type=&quot;module&quot;&gt;&lt;/script&gt;&lt;/body&gt;
&lt;/html&gt;
</code></pre>
				
				
			</details></td>
	</tr>
	
	
	
	<tr>
		<th scope="row">Solution</th>
		<td> 
<p>Ensure that your web server, application server, load balancer, etc. is configured to set the Content-Security-Policy header.</p>
 </td>
	</tr>
</table>

											</details></li>
									</ol>
								</li>
								
							</ol>
						</li>
						
					</ol>
				</li>
				
				<li id="alerts--risk-2-confidence-2">
					<h3>
						<span>Risk</span>=<span
							class="risk-level">Médio</span>, <span>Confidence</span>=<span
							class="confidence-level">Médio</span> <span>(1)</span>
					</h3>
					<ol>
						
						<li class="alerts--site-li">
							<h4>
								<span class="site">https://juice-shop.herokuapp.com</span> <span>(1)</span>
							</h4>
							<ol>
								
								<li>
									<h5>
										<a
											href="#alert-type-0">Configuração Incorreta Entre Domínios</a> <span>(1)</span>
									</h5>
									<ol>
										<li><details>
												<summary>
													<span class="request-method-n-url">GET https://juice-shop.herokuapp.com/robots.txt</span>
												</summary>
												
<table class="alerts-table">
	<tr>
		<th scope="row">Alert tags</th>
		<td>
			<ul class="alert-tags-list">
				<li>
					<span><a href="https://owasp.org/Top10/A01_2021-Broken_Access_Control/">OWASP_2021_A01</a></span> 
				</li>
				<li>
					 <span>POLICY_QA_STD = </span>
				</li>
				<li>
					 <span>POLICY_PENTEST = </span>
				</li>
				<li>
					<span><a href="https://www.zaproxy.org/docs/desktop/addons/common-library/alerttags/#systemic">SYSTEMIC</a></span> 
				</li>
				<li>
					<span><a href="https://owasp.org/Top10/2025/A01_2025-Broken_Access_Control/">OWASP_2025_A01</a></span> 
				</li>
				<li>
					<span><a href="https://owasp.org/www-project-top-ten/2017/A5_2017-Broken_Access_Control.html">OWASP_2017_A05</a></span> 
				</li>
				<li>
					<span><a href="https://cwe.mitre.org/data/definitions/264.html">CWE-264</a></span> 
				</li>
			</ul>
		</td>
	</tr>
	<tr>
		<th scope="row">Alert description</th>
		<td> 
<p>Web browser data loading may be possible, due to a Cross Origin Resource Sharing (CORS) misconfiguration on the web server.</p>
 </td>
	</tr>
	<tr>
		<th scope="row">Other info</th>
		<td> 
<p>A configuração incorreta do CORS no servidor da web permite solicitações de leitura entre domínios de domínios arbitrários de terceiros, usando APIs não autenticadas neste domínio. No entanto, as implementações do navegador da Web não permitem que terceiros arbitrários leiam a resposta de APIs autenticadas. Isso reduz o risco um pouco. Essa configuração incorreta pode ser usada por um invasor para acessar dados que estão disponíveis de maneira não autenticada, mas que usam alguma outra forma de segurança, como lista de permissões de endereços IP.</p>
 </td>
	</tr>
	<tr>
		<th scope="row">Request</th>
		<td><details open="open">
				<summary>Request line and header section (260 bytes)</summary>
				
				<pre><code>GET https://juice-shop.herokuapp.com/robots.txt HTTP/1.1
host: juice-shop.herokuapp.com
user-agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/141.0.0.0 Safari/537.36
pragma: no-cache
cache-control: no-cache

</code></pre>
				
				
			</details> <details class="request-body" open="open">
				<summary>Request body (0 bytes)</summary>
				
				<pre><code></code></pre>
				
				
			</details></td>
	</tr>
	<tr>
		<th scope="row">Response</th>
		<td><details open="open">
				<summary>Status line and header section (890 bytes)</summary>
				
				<pre><code>HTTP/1.1 200 OK
Access-Control-Allow-Origin: *
Content-Length: 28
Content-Type: text/plain; charset=utf-8
Date: Wed, 12 Aug 2026 19:28:49 GMT
Etag: W/&quot;1c-8HgF6mNyhsSFK0pascC9uB0wjX0&quot;
Feature-Policy: payment &#39;self&#39;
Nel: {&quot;report_to&quot;:&quot;heroku-nel&quot;,&quot;response_headers&quot;:[&quot;Via&quot;],&quot;max_age&quot;:3600,&quot;success_fraction&quot;:0.01,&quot;failure_fraction&quot;:0.1}
Report-To: {&quot;group&quot;:&quot;heroku-nel&quot;,&quot;endpoints&quot;:[{&quot;url&quot;:&quot;https://nel.heroku.com/reports?s=XJ%2BSWZNyTjZrXjtc88LQgL%2FIIBv5MD1mBAlUm4hYMKE%3D\u0026sid=812dcc77-0bd0-43b1-a5f1-b25750382959\u0026ts=1786562929&quot;}],&quot;max_age&quot;:3600}
Reporting-Endpoints: heroku-nel=&quot;https://nel.heroku.com/reports?s=XJ%2BSWZNyTjZrXjtc88LQgL%2FIIBv5MD1mBAlUm4hYMKE%3D&amp;sid=812dcc77-0bd0-43b1-a5f1-b25750382959&amp;ts=1786562929&quot;
Server: Heroku
Vary: Accept-Encoding
Via: 1.1 heroku-router
X-Content-Type-Options: nosniff
X-Frame-Options: SAMEORIGIN
X-Recruiting: /#/jobs

</code></pre>
				
				
			</details> <details class="response-body" open="open">
				<summary>Response body (28 bytes)</summary>
				
				<pre><code>User-agent: *
Disallow: /ftp</code></pre>
				
				
			</details></td>
	</tr>
	
	
	<tr>
		<th scope="row">Evidence</th>
		<td><pre><code>Access-Control-Allow-Origin: *</code></pre></td>
	</tr>
	<tr>
		<th scope="row">Solution</th>
		<td> 
<p>Certifique-se de que dados confidenciais não estejam disponíveis de maneira não autenticada (usando uma lista branca/de permissões de endereços IP, por exemplo). Configure o cabeçalho HTTP &quot;Access-Control-Allow-Origin&quot; para um conjunto mais restritivo de domínios ou remova todos os cabeçalhos CORS inteiramente, para permitir que o navegador web aplique a Same Origin Policy (SOP) de uma maneira mais restritiva.</p>
 </td>
	</tr>
</table>

											</details></li>
									</ol>
								</li>
								
							</ol>
						</li>
						
					</ol>
				</li>
				
				
				  
				 
				
				
				<li id="alerts--risk-1-confidence-3">
					<h3>
						<span>Risk</span>=<span
							class="risk-level">Baixo</span>, <span>Confidence</span>=<span
							class="confidence-level">Alto</span> <span>(1)</span>
					</h3>
					<ol>
						
						<li class="alerts--site-li">
							<h4>
								<span class="site">https://juice-shop.herokuapp.com</span> <span>(1)</span>
							</h4>
							<ol>
								
								<li>
									<h5>
										<a
											href="#alert-type-3">Strict-Transport-Security Header Not Set</a> <span>(1)</span>
									</h5>
									<ol>
										<li><details>
												<summary>
													<span class="request-method-n-url">GET https://juice-shop.herokuapp.com/robots.txt</span>
												</summary>
												
<table class="alerts-table">
	<tr>
		<th scope="row">Alert tags</th>
		<td>
			<ul class="alert-tags-list">
				<li>
					<span><a href="https://owasp.org/Top10/A05_2021-Security_Misconfiguration/">OWASP_2021_A05</a></span> 
				</li>
				<li>
					 <span>POLICY_QA_STD = </span>
				</li>
				<li>
					 <span>POLICY_PENTEST = </span>
				</li>
				<li>
					<span><a href="https://www.zaproxy.org/docs/desktop/addons/common-library/alerttags/#systemic">SYSTEMIC</a></span> 
				</li>
				<li>
					<span><a href="https://cwe.mitre.org/data/definitions/319.html">CWE-319</a></span> 
				</li>
				<li>
					<span><a href="https://owasp.org/www-project-top-ten/2017/A6_2017-Security_Misconfiguration.html">OWASP_2017_A06</a></span> 
				</li>
				<li>
					<span><a href="https://owasp.org/Top10/2025/A02_2025-Security_Misconfiguration/">OWASP_2025_A02</a></span> 
				</li>
			</ul>
		</td>
	</tr>
	<tr>
		<th scope="row">Alert description</th>
		<td> 
<p>HTTP Strict Transport Security (HSTS) is a web security policy mechanism whereby a web server declares that complying user agents (such as a web browser) are to interact with it using only secure HTTPS connections (i.e. HTTP layered over TLS/SSL). HSTS is an IETF standards track protocol and is specified in RFC 6797.</p>
 </td>
	</tr>
	
	<tr>
		<th scope="row">Request</th>
		<td><details open="open">
				<summary>Request line and header section (260 bytes)</summary>
				
				<pre><code>GET https://juice-shop.herokuapp.com/robots.txt HTTP/1.1
host: juice-shop.herokuapp.com
user-agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/141.0.0.0 Safari/537.36
pragma: no-cache
cache-control: no-cache

</code></pre>
				
				
			</details> <details class="request-body" open="open">
				<summary>Request body (0 bytes)</summary>
				
				<pre><code></code></pre>
				
				
			</details></td>
	</tr>
	<tr>
		<th scope="row">Response</th>
		<td><details open="open">
				<summary>Status line and header section (890 bytes)</summary>
				
				<pre><code>HTTP/1.1 200 OK
Access-Control-Allow-Origin: *
Content-Length: 28
Content-Type: text/plain; charset=utf-8
Date: Wed, 12 Aug 2026 19:28:49 GMT
Etag: W/&quot;1c-8HgF6mNyhsSFK0pascC9uB0wjX0&quot;
Feature-Policy: payment &#39;self&#39;
Nel: {&quot;report_to&quot;:&quot;heroku-nel&quot;,&quot;response_headers&quot;:[&quot;Via&quot;],&quot;max_age&quot;:3600,&quot;success_fraction&quot;:0.01,&quot;failure_fraction&quot;:0.1}
Report-To: {&quot;group&quot;:&quot;heroku-nel&quot;,&quot;endpoints&quot;:[{&quot;url&quot;:&quot;https://nel.heroku.com/reports?s=XJ%2BSWZNyTjZrXjtc88LQgL%2FIIBv5MD1mBAlUm4hYMKE%3D\u0026sid=812dcc77-0bd0-43b1-a5f1-b25750382959\u0026ts=1786562929&quot;}],&quot;max_age&quot;:3600}
Reporting-Endpoints: heroku-nel=&quot;https://nel.heroku.com/reports?s=XJ%2BSWZNyTjZrXjtc88LQgL%2FIIBv5MD1mBAlUm4hYMKE%3D&amp;sid=812dcc77-0bd0-43b1-a5f1-b25750382959&amp;ts=1786562929&quot;
Server: Heroku
Vary: Accept-Encoding
Via: 1.1 heroku-router
X-Content-Type-Options: nosniff
X-Frame-Options: SAMEORIGIN
X-Recruiting: /#/jobs

</code></pre>
				
				
			</details> <details class="response-body" open="open">
				<summary>Response body (28 bytes)</summary>
				
				<pre><code>User-agent: *
Disallow: /ftp</code></pre>
				
				
			</details></td>
	</tr>
	
	
	
	<tr>
		<th scope="row">Solution</th>
		<td> 
<p>Ensure that your web server, application server, load balancer, etc. is configured to enforce Strict-Transport-Security.</p>
 </td>
	</tr>
</table>

											</details></li>
									</ol>
								</li>
								
							</ol>
						</li>
						
					</ol>
				</li>
				
				
				
				<li id="alerts--risk-1-confidence-1">
					<h3>
						<span>Risk</span>=<span
							class="risk-level">Baixo</span>, <span>Confidence</span>=<span
							class="confidence-level">Baixo</span> <span>(1)</span>
					</h3>
					<ol>
						
						<li class="alerts--site-li">
							<h4>
								<span class="site">https://juice-shop.herokuapp.com</span> <span>(1)</span>
							</h4>
							<ol>
								
								<li>
									<h5>
										<a
											href="#alert-type-2">Divulgação de Data e Hora - Unix</a> <span>(1)</span>
									</h5>
									<ol>
										<li><details>
												<summary>
													<span class="request-method-n-url">GET https://juice-shop.herokuapp.com/robots.txt</span>
												</summary>
												
<table class="alerts-table">
	<tr>
		<th scope="row">Alert tags</th>
		<td>
			<ul class="alert-tags-list">
				<li>
					<span><a href="https://owasp.org/Top10/A01_2021-Broken_Access_Control/">OWASP_2021_A01</a></span> 
				</li>
				<li>
					<span><a href="https://owasp.org/www-project-top-ten/2017/A3_2017-Sensitive_Data_Exposure.html">OWASP_2017_A03</a></span> 
				</li>
				<li>
					<span><a href="https://owasp.org/Top10/2025/A01_2025-Broken_Access_Control/">OWASP_2025_A01</a></span> 
				</li>
				<li>
					 <span>POLICY_PENTEST = </span>
				</li>
				<li>
					<span><a href="https://cwe.mitre.org/data/definitions/497.html">CWE-497</a></span> 
				</li>
				<li>
					<span><a href="https://www.zaproxy.org/docs/desktop/addons/common-library/alerttags/#systemic">SYSTEMIC</a></span> 
				</li>
			</ul>
		</td>
	</tr>
	<tr>
		<th scope="row">Alert description</th>
		<td> 
<p>A timestamp was disclosed by the application/web server. - Unix</p>
 </td>
	</tr>
	<tr>
		<th scope="row">Other info</th>
		<td> 
<p>1786562929, which evaluates to: 2026-08-12 16:28:49.</p>
 </td>
	</tr>
	<tr>
		<th scope="row">Request</th>
		<td><details open="open">
				<summary>Request line and header section (260 bytes)</summary>
				
				<pre><code>GET https://juice-shop.herokuapp.com/robots.txt HTTP/1.1
host: juice-shop.herokuapp.com
user-agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/141.0.0.0 Safari/537.36
pragma: no-cache
cache-control: no-cache

</code></pre>
				
				
			</details> <details class="request-body" open="open">
				<summary>Request body (0 bytes)</summary>
				
				<pre><code></code></pre>
				
				
			</details></td>
	</tr>
	<tr>
		<th scope="row">Response</th>
		<td><details open="open">
				<summary>Status line and header section (890 bytes)</summary>
				
				<pre><code>HTTP/1.1 200 OK
Access-Control-Allow-Origin: *
Content-Length: 28
Content-Type: text/plain; charset=utf-8
Date: Wed, 12 Aug 2026 19:28:49 GMT
Etag: W/&quot;1c-8HgF6mNyhsSFK0pascC9uB0wjX0&quot;
Feature-Policy: payment &#39;self&#39;
Nel: {&quot;report_to&quot;:&quot;heroku-nel&quot;,&quot;response_headers&quot;:[&quot;Via&quot;],&quot;max_age&quot;:3600,&quot;success_fraction&quot;:0.01,&quot;failure_fraction&quot;:0.1}
Report-To: {&quot;group&quot;:&quot;heroku-nel&quot;,&quot;endpoints&quot;:[{&quot;url&quot;:&quot;https://nel.heroku.com/reports?s=XJ%2BSWZNyTjZrXjtc88LQgL%2FIIBv5MD1mBAlUm4hYMKE%3D\u0026sid=812dcc77-0bd0-43b1-a5f1-b25750382959\u0026ts=1786562929&quot;}],&quot;max_age&quot;:3600}
Reporting-Endpoints: heroku-nel=&quot;https://nel.heroku.com/reports?s=XJ%2BSWZNyTjZrXjtc88LQgL%2FIIBv5MD1mBAlUm4hYMKE%3D&amp;sid=812dcc77-0bd0-43b1-a5f1-b25750382959&amp;ts=1786562929&quot;
Server: Heroku
Vary: Accept-Encoding
Via: 1.1 heroku-router
X-Content-Type-Options: nosniff
X-Frame-Options: SAMEORIGIN
X-Recruiting: /#/jobs

</code></pre>
				
				
			</details> <details class="response-body" open="open">
				<summary>Response body (28 bytes)</summary>
				
				<pre><code>User-agent: *
Disallow: /ftp</code></pre>
				
				
			</details></td>
	</tr>
	<tr>
		<th scope="row">Parameter</th>
		<td><pre><code>Reporting-Endpoints</code></pre></td>
	</tr>
	
	<tr>
		<th scope="row">Evidence</th>
		<td><pre><code>1786562929</code></pre></td>
	</tr>
	<tr>
		<th scope="row">Solution</th>
		<td> 
<p>Confirme manualmente se os dados do carimbo de data/hora não são confidenciais e se os dados não podem ser agregados para divulgar padrões exploráveis.</p>
 </td>
	</tr>
</table>

											</details></li>
									</ol>
								</li>
								
							</ol>
						</li>
						
					</ol>
				</li>
				  
				 
				
				
				
				
				<li id="alerts--risk-0-confidence-2">
					<h3>
						<span>Risk</span>=<span
							class="risk-level">Informativo</span>, <span>Confidence</span>=<span
							class="confidence-level">Médio</span> <span>(1)</span>
					</h3>
					<ol>
						
						<li class="alerts--site-li">
							<h4>
								<span class="site">https://juice-shop.herokuapp.com</span> <span>(1)</span>
							</h4>
							<ol>
								
								<li>
									<h5>
										<a
											href="#alert-type-4">Modern Web Application</a> <span>(1)</span>
									</h5>
									<ol>
										<li><details>
												<summary>
													<span class="request-method-n-url">GET https://juice-shop.herokuapp.com</span>
												</summary>
												
<table class="alerts-table">
	<tr>
		<th scope="row">Alert tags</th>
		<td>
			<ul class="alert-tags-list">
				<li>
					 <span>POLICY_QA_STD = </span>
				</li>
				<li>
					 <span>POLICY_PENTEST = </span>
				</li>
				<li>
					<span><a href="https://www.zaproxy.org/docs/desktop/addons/common-library/alerttags/#systemic">SYSTEMIC</a></span> 
				</li>
				<li>
					 <span>POLICY_DEV_STD = </span>
				</li>
			</ul>
		</td>
	</tr>
	<tr>
		<th scope="row">Alert description</th>
		<td> 
<p>The application appears to be a modern web application. If you need to explore it automatically then the Ajax Spider may well be more effective than the standard one.</p>
 </td>
	</tr>
	<tr>
		<th scope="row">Other info</th>
		<td> 
<p>No links have been found while there are scripts, which is an indication that this is a modern web application.</p>
 </td>
	</tr>
	<tr>
		<th scope="row">Request</th>
		<td><details open="open">
				<summary>Request line and header section (249 bytes)</summary>
				
				<pre><code>GET https://juice-shop.herokuapp.com HTTP/1.1
host: juice-shop.herokuapp.com
user-agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/141.0.0.0 Safari/537.36
pragma: no-cache
cache-control: no-cache

</code></pre>
				
				
			</details> <details class="request-body" open="open">
				<summary>Request body (0 bytes)</summary>
				
				<pre><code></code></pre>
				
				
			</details></td>
	</tr>
	<tr>
		<th scope="row">Response</th>
		<td><details open="open">
				<summary>Status line and header section (971 bytes)</summary>
				
				<pre><code>HTTP/1.1 200 OK
Accept-Ranges: bytes
Access-Control-Allow-Origin: *
Cache-Control: public, max-age=0
Content-Length: 9393
Content-Type: text/html; charset=UTF-8
Date: Wed, 12 Aug 2026 19:28:50 GMT
Etag: W/&quot;24b1-19ff772555c&quot;
Feature-Policy: payment &#39;self&#39;
Last-Modified: Wed, 12 Aug 2026 19:28:13 GMT
Nel: {&quot;report_to&quot;:&quot;heroku-nel&quot;,&quot;response_headers&quot;:[&quot;Via&quot;],&quot;max_age&quot;:3600,&quot;success_fraction&quot;:0.01,&quot;failure_fraction&quot;:0.1}
Report-To: {&quot;group&quot;:&quot;heroku-nel&quot;,&quot;endpoints&quot;:[{&quot;url&quot;:&quot;https://nel.heroku.com/reports?s=A7nLX6H2ffs1o5K7ohmdiVi9fjMxMvRv2P8tsq8GCrU%3D\u0026sid=812dcc77-0bd0-43b1-a5f1-b25750382959\u0026ts=1786562930&quot;}],&quot;max_age&quot;:3600}
Reporting-Endpoints: heroku-nel=&quot;https://nel.heroku.com/reports?s=A7nLX6H2ffs1o5K7ohmdiVi9fjMxMvRv2P8tsq8GCrU%3D&amp;sid=812dcc77-0bd0-43b1-a5f1-b25750382959&amp;ts=1786562930&quot;
Server: Heroku
Vary: Accept-Encoding
Via: 1.1 heroku-router
X-Content-Type-Options: nosniff
X-Frame-Options: SAMEORIGIN
X-Recruiting: /#/jobs

</code></pre>
				
				
			</details> <details class="response-body">
				<summary>Response body (9393 bytes)</summary>
				
				<pre><code>&lt;!--
  ~ Copyright (c) 2014-2026 Bjoern Kimminich &amp; the OWASP Juice Shop contributors.
  ~ SPDX-License-Identifier: MIT
  --&gt;

&lt;!doctype html&gt;
&lt;html lang=&quot;en&quot; data-beasties-container&gt;
&lt;head&gt;
  &lt;meta charset=&quot;utf-8&quot;&gt;
  &lt;title&gt;OWASP Juice Shop&lt;/title&gt;
  &lt;meta name=&quot;description&quot; content=&quot;Probably the most modern and sophisticated insecure web application&quot;&gt;
  &lt;meta name=&quot;viewport&quot; content=&quot;width=device-width, initial-scale=1&quot;&gt;
  &lt;link rel=&quot;preconnect&quot; href=&quot;https://fonts.googleapis.com&quot;&gt;
  &lt;link rel=&quot;preconnect&quot; href=&quot;https://fonts.gstatic.com&quot; crossorigin&gt;
  &lt;style&gt;@font-face{font-family:&#39;VT323&#39;;font-style:normal;font-weight:400;font-display:swap;src:url(https://fonts.gstatic.com/s/vt323/v18/pxiKyp0ihIEF2isQFJXGdg.woff2) format(&#39;woff2&#39;);unicode-range:U+0102-0103, U+0110-0111, U+0128-0129, U+0168-0169, U+01A0-01A1, U+01AF-01B0, U+0300-0301, U+0303-0304, U+0308-0309, U+0323, U+0329, U+1EA0-1EF9, U+20AB;}@font-face{font-family:&#39;VT323&#39;;font-style:normal;font-weight:400;font-display:swap;src:url(https://fonts.gstatic.com/s/vt323/v18/pxiKyp0ihIEF2isRFJXGdg.woff2) format(&#39;woff2&#39;);unicode-range:U+0100-02BA, U+02BD-02C5, U+02C7-02CC, U+02CE-02D7, U+02DD-02FF, U+0304, U+0308, U+0329, U+1D00-1DBF, U+1E00-1E9F, U+1EF2-1EFF, U+2020, U+20A0-20AB, U+20AD-20C0, U+2113, U+2C60-2C7F, U+A720-A7FF;}@font-face{font-family:&#39;VT323&#39;;font-style:normal;font-weight:400;font-display:swap;src:url(https://fonts.gstatic.com/s/vt323/v18/pxiKyp0ihIEF2isfFJU.woff2) format(&#39;woff2&#39;);unicode-range:U+0000-00FF, U+0131, U+0152-0153, U+02BB-02BC, U+02C6, U+02DA, U+02DC, U+0304, U+0308, U+0329, U+2000-206F, U+20AC, U+2122, U+2191, U+2193, U+2212, U+2215, U+FEFF, U+FFFD;}&lt;/style&gt;
  &lt;link id=&quot;favicon&quot; rel=&quot;icon&quot; type=&quot;image/x-icon&quot; href=&quot;assets/public/favicon_js.ico&quot;&gt;
  &lt;script&gt;
    window.addEventListener(&quot;load&quot;, function(){
      window.cookieconsent.initialise({
        &quot;palette&quot;: {
          &quot;popup&quot;: { &quot;background&quot;: &quot;var(--theme-primary)&quot;, &quot;text&quot;: &quot;var(--theme-text)&quot; },
          &quot;button&quot;: { &quot;background&quot;: &quot;var(--theme-accent)&quot;, &quot;text&quot;: &quot;var(--theme-text)&quot; }
        },
        &quot;theme&quot;: &quot;classic&quot;,
        &quot;position&quot;: &quot;bottom-right&quot;,
        &quot;content&quot;: { &quot;message&quot;: &quot;This website uses fruit cookies to ensure you get the juiciest tracking experience.&quot;, &quot;dismiss&quot;: &quot;Me want it!&quot;, &quot;link&quot;: &quot;But me wait!&quot;, &quot;href&quot;: &quot;https://www.youtube.com/watch?v=9PnbKL3wuH4&quot; }
      })});
  &lt;/script&gt;
&lt;style&gt;.bluegrey-lightgreen-theme{--mat-sys-background:#121316;--mat-sys-error:#ffb4ab;--mat-sys-error-container:#93000a;--mat-sys-inverse-on-surface:#2f3033;--mat-sys-inverse-primary:#005cbb;--mat-sys-inverse-surface:#e3e2e6;--mat-sys-on-background:#e3e2e6;--mat-sys-on-error:#690005;--mat-sys-on-error-container:#ffdad6;--mat-sys-on-primary:#002f65;--mat-sys-on-primary-container:#d7e3ff;--mat-sys-on-primary-fixed:#001b3f;--mat-sys-on-primary-fixed-variant:#00458f;--mat-sys-on-secondary:#283041;--mat-sys-on-secondary-container:#dae2f9;--mat-sys-on-secondary-fixed:#131c2b;--mat-sys-on-secondary-fixed-variant:#3e4759;--mat-sys-on-surface:#e3e2e6;--mat-sys-on-surface-variant:#e0e2ec;--mat-sys-on-tertiary:#173800;--mat-sys-on-tertiary-container:#82ff10;--mat-sys-on-tertiary-fixed:#0b2000;--mat-sys-on-tertiary-fixed-variant:#245100;--mat-sys-outline:#8e9099;--mat-sys-outline-variant:#44474e;--mat-sys-primary:#abc7ff;--mat-sys-primary-container:#00458f;--mat-sys-primary-fixed:#d7e3ff;--mat-sys-primary-fixed-dim:#abc7ff;--mat-sys-scrim:#000000;--mat-sys-secondary:#bec6dc;--mat-sys-secondary-container:#3e4759;--mat-sys-secondary-fixed:#dae2f9;--mat-sys-secondary-fixed-dim:#bec6dc;--mat-sys-shadow:#000000;--mat-sys-surface:#121316;--mat-sys-surface-bright:#38393c;--mat-sys-surface-container:#1f2022;--mat-sys-surface-container-high:#292a2c;--mat-sys-surface-container-highest:#343537;--mat-sys-surface-container-low:#1a1b1f;--mat-sys-surface-container-lowest:#0d0e11;--mat-sys-surface-dim:#121316;--mat-sys-surface-tint:#abc7ff;--mat-sys-surface-variant:#44474e;--mat-sys-tertiary:#70e000;--mat-sys-tertiary-container:#245100;--mat-sys-tertiary-fixed:#82ff10;--mat-sys-tertiary-fixed-dim:#70e000;--mat-sys-neutral-variant20:#2d3038;--mat-sys-neutral10:#1a1b1f;--mat-sys-level0:0px 0px 0px 0px rgba(0, 0, 0, .2), 0px 0px 0px 0px rgba(0, 0, 0, .14), 0px 0px 0px 0px rgba(0, 0, 0, .12);--mat-sys-level1:0px 2px 1px -1px rgba(0, 0, 0, .2), 0px 1px 1px 0px rgba(0, 0, 0, .14), 0px 1px 3px 0px rgba(0, 0, 0, .12);--mat-sys-level2:0px 3px 3px -2px rgba(0, 0, 0, .2), 0px 3px 4px 0px rgba(0, 0, 0, .14), 0px 1px 8px 0px rgba(0, 0, 0, .12);--mat-sys-level3:0px 3px 5px -1px rgba(0, 0, 0, .2), 0px 6px 10px 0px rgba(0, 0, 0, .14), 0px 1px 18px 0px rgba(0, 0, 0, .12);--mat-sys-level4:0px 5px 5px -3px rgba(0, 0, 0, .2), 0px 8px 10px 1px rgba(0, 0, 0, .14), 0px 3px 14px 2px rgba(0, 0, 0, .12);--mat-sys-level5:0px 7px 8px -4px rgba(0, 0, 0, .2), 0px 12px 17px 2px rgba(0, 0, 0, .14), 0px 5px 22px 4px rgba(0, 0, 0, .12);--mat-sys-corner-extra-large:28px;--mat-sys-corner-extra-large-top:28px 28px 0 0;--mat-sys-corner-extra-small:4px;--mat-sys-corner-extra-small-top:4px 4px 0 0;--mat-sys-corner-full:9999px;--mat-sys-corner-large:16px;--mat-sys-corner-large-end:0 16px 16px 0;--mat-sys-corner-large-start:16px 0 0 16px;--mat-sys-corner-large-top:16px 16px 0 0;--mat-sys-corner-medium:12px;--mat-sys-corner-none:0;--mat-sys-corner-small:8px;--mat-sys-dragged-state-layer-opacity:.16;--mat-sys-focus-state-layer-opacity:.12;--mat-sys-hover-state-layer-opacity:.08;--mat-sys-pressed-state-layer-opacity:.12;color:var(--mat-sys-on-surface);background-color:var(--mat-sys-surface)}html{font-family:var(--mat-sys-body-medium-font, Roboto, &quot;Helvetica Neue&quot;, sans-serif)}.bluegrey-lightgreen-theme{--theme-primary:#438fff;--theme-primary-lighter:rgb(97.6, 161.229787234, 255);--theme-primary-light:rgb(118, 173.3829787234, 255);--theme-primary-darker:rgb(36.4, 124.770212766, 255);--theme-primary-dark:rgb(16, 112.6170212766, 255);--theme-primary-fade-10:#438fff;--theme-primary-fade-20:#438fff;--theme-primary-fade-30:#438fff;--theme-primary-fade-40:#438fff;--theme-primary-fade-50:#438fff;--theme-accent:#50a400;--theme-accent-lighter:rgb(94.9268292683, 194.6, 0);--theme-accent-light:rgb(104.8780487805, 215, 0);--theme-accent-darker:rgb(65.0731707317, 133.4, 0);--theme-accent-dark:rgb(55.1219512195, 113, 0);--theme-accent-fade-10:#50a400;--theme-accent-fade-20:#50a400;--theme-accent-fade-30:#50a400;--theme-accent-fade-40:#50a400;--theme-accent-fade-50:#50a400;--theme-warn:#ffb4ab;--theme-warn-lighter:rgb(255, 207.3214285714, 201.6);--theme-warn-light:rgb(255, 225.5357142857, 222);--theme-warn-darker:rgb(255, 152.6785714286, 140.4);--theme-warn-dark:rgb(255, 134.4642857143, 120);--theme-warn-fade-10:#ffb4ab;--theme-warn-fade-20:#ffb4ab;--theme-warn-fade-30:#ffb4ab;--theme-warn-fade-40:#ffb4ab;--theme-warn-fade-50:#ffb4ab;--theme-text:#e3e2e6;--theme-text-lighter:rgb(242.8666666667, 242.4333333333, 244.1666666667);--theme-text-light:rgb(253.4444444444, 253.3888888889, 253.6111111111);--theme-text-darker:rgb(200.5555555556, 198.6111111111, 206.3888888889);--theme-text-dark:rgb(160.8888888889, 157.5277777778, 170.9722222222);--theme-text-fade-10:#e3e2e6;--theme-text-fade-20:#e3e2e6;--theme-text-fade-30:#e3e2e6;--theme-text-fade-40:#e3e2e6;--theme-text-fade-50:#e3e2e6;--theme-text-invert-15:rgb(197.15, 196.45, 199.25);--theme-text-invert-30:rgb(167.3, 166.9, 168.5);--theme-background:#1f2022;--theme-background-lighter:rgb(45.5938461538, 47.0646153846, 50.0061538462);--theme-background-light:rgb(55.3230769231, 57.1076923077, 60.6769230769);--theme-background-darker:rgb(16.4061538462, 16.9353846154, 17.9938461538);--theme-background-dark:rgb(6.6769230769, 6.8923076923, 7.3230769231);--theme-background-darkest:hsl(220, 4.6153846154%, -1.2549019608%);--theme-thumbnail-border:1px solid #abc7ff;--mdc-filled-text-field-container-color:#0000;--mdc-filled-text-field-disabled-container-color:#0000;--theme-background:#3e3e3e;--theme-background-lighter:#4a4a4a;--theme-background-light:#5a5a5a;--theme-background-darker:#333638;--theme-background-dark:#303030;--theme-background-darkest:#2b2b2b;--theme-text:#e8ecef;--theme-text-lighter:#f2f5f7;--theme-text-light:#fff;--theme-text-darker:#b8c0c7;--theme-text-dark:#7f8a93;--mat-sys-surface:#333638;--mat-sys-on-surface:#e8ecef;--mat-sys-surface-container:#3e3e3e;--mat-sys-surface-container-high:#404244;--mat-sys-on-surface-variant:#b8c0c7;--mat-sys-outline:#5a5a5a;--mat-sys-outline-variant:#404244}.bluegrey-lightgreen-theme{--theme-warn:#f44336;--theme-warn-lighter:rgb(245.5877358491, 94.1358490566, 83.0122641509);--theme-warn-light:rgb(246.6462264151, 112.2264150943, 102.3537735849);--theme-warn-darker:rgb(242.4122641509, 39.8641509434, 24.9877358491);--theme-warn-dark:rgb(234.1839622642, 27.9622641509, 12.8160377358);--theme-warn-fade-10:#f44336;--theme-warn-fade-20:#f44336;--theme-warn-fade-30:#f44336;--theme-warn-fade-40:#f44336;--theme-warn-fade-50:#f44336;--mat-sys-error:#f44336;--mat-sys-on-error:#fff}@media screen and (-webkit-min-device-pixel-ratio:0){}&lt;/style&gt;&lt;link rel=&quot;stylesheet&quot; href=&quot;styles.css&quot; media=&quot;print&quot; onload=&quot;this.media=&#39;all&#39;&quot;&gt;&lt;noscript&gt;&lt;link rel=&quot;stylesheet&quot; href=&quot;styles.css&quot;&gt;&lt;/noscript&gt;&lt;/head&gt;
&lt;body class=&quot;bluegrey-lightgreen-theme&quot;&gt;
  &lt;app-root&gt;&lt;/app-root&gt;
&lt;script src=&quot;polyfills.js&quot; type=&quot;module&quot;&gt;&lt;/script&gt;&lt;script src=&quot;scripts.js&quot; defer&gt;&lt;/script&gt;&lt;script src=&quot;main.js&quot; type=&quot;module&quot;&gt;&lt;/script&gt;&lt;/body&gt;
&lt;/html&gt;
</code></pre>
				
				
			</details></td>
	</tr>
	
	
	<tr>
		<th scope="row">Evidence</th>
		<td><pre><code>&lt;script&gt;
    window.addEventListener(&quot;load&quot;, function(){
      window.cookieconsent.initialise({
        &quot;palette&quot;: {
          &quot;popup&quot;: { &quot;background&quot;: &quot;var(--theme-primary)&quot;, &quot;text&quot;: &quot;var(--theme-text)&quot; },
          &quot;button&quot;: { &quot;background&quot;: &quot;var(--theme-accent)&quot;, &quot;text&quot;: &quot;var(--theme-text)&quot; }
        },
        &quot;theme&quot;: &quot;classic&quot;,
        &quot;position&quot;: &quot;bottom-right&quot;,
        &quot;content&quot;: { &quot;message&quot;: &quot;This website uses fruit cookies to ensure you get the juiciest tracking experience.&quot;, &quot;dismiss&quot;: &quot;Me want it!&quot;, &quot;link&quot;: &quot;But me wait!&quot;, &quot;href&quot;: &quot;https://www.youtube.com/watch?v=9PnbKL3wuH4&quot; }
      })});
  &lt;/script&gt;</code></pre></td>
	</tr>
	<tr>
		<th scope="row">Solution</th>
		<td> 
<p>This is an informational alert and so no changes are required.</p>
 </td>
	</tr>
</table>

											</details></li>
									</ol>
								</li>
								
							</ol>
						</li>
						
					</ol>
				</li>
				
				<li id="alerts--risk-0-confidence-1">
					<h3>
						<span>Risk</span>=<span
							class="risk-level">Informativo</span>, <span>Confidence</span>=<span
							class="confidence-level">Baixo</span> <span>(1)</span>
					</h3>
					<ol>
						
						<li class="alerts--site-li">
							<h4>
								<span class="site">https://juice-shop.herokuapp.com</span> <span>(1)</span>
							</h4>
							<ol>
								
								<li>
									<h5>
										<a
											href="#alert-type-5">Re-examine Cache-control Directives</a> <span>(1)</span>
									</h5>
									<ol>
										<li><details>
												<summary>
													<span class="request-method-n-url">GET https://juice-shop.herokuapp.com/robots.txt</span>
												</summary>
												
<table class="alerts-table">
	<tr>
		<th scope="row">Alert tags</th>
		<td>
			<ul class="alert-tags-list">
				<li>
					<span><a href="https://owasp.org/Top10/2025/A07_2025-Authentication_Failures/">OWASP_2025_A07</a></span> 
				</li>
				<li>
					<span><a href="https://cwe.mitre.org/data/definitions/525.html">CWE-525</a></span> 
				</li>
				<li>
					 <span>POLICY_PENTEST = </span>
				</li>
				<li>
					<span><a href="https://owasp.org/Top10/A07_2021-Identification_and_Authentication_Failures/">OWASP_2021_A07</a></span> 
				</li>
				<li>
					<span><a href="https://www.zaproxy.org/docs/desktop/addons/common-library/alerttags/#systemic">SYSTEMIC</a></span> 
				</li>
				<li>
					<span><a href="https://owasp.org/www-project-web-security-testing-guide/v42/4-Web_Application_Security_Testing/04-Authentication_Testing/06-Testing_for_Browser_Cache_Weaknesses">WSTG-v42-ATHN-06</a></span> 
				</li>
				<li>
					<span><a href="https://owasp.org/www-project-top-ten/2017/A2_2017-Broken_Authentication.html">OWASP_2017_A02</a></span> 
				</li>
			</ul>
		</td>
	</tr>
	<tr>
		<th scope="row">Alert description</th>
		<td> 
<p>The cache-control header has not been set properly or is missing, allowing the browser and proxies to cache content. For static assets like css, js, or image files this might be intended, however, the resources should be reviewed to ensure that no sensitive content will be cached.</p>
 </td>
	</tr>
	
	<tr>
		<th scope="row">Request</th>
		<td><details open="open">
				<summary>Request line and header section (260 bytes)</summary>
				
				<pre><code>GET https://juice-shop.herokuapp.com/robots.txt HTTP/1.1
host: juice-shop.herokuapp.com
user-agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/141.0.0.0 Safari/537.36
pragma: no-cache
cache-control: no-cache

</code></pre>
				
				
			</details> <details class="request-body" open="open">
				<summary>Request body (0 bytes)</summary>
				
				<pre><code></code></pre>
				
				
			</details></td>
	</tr>
	<tr>
		<th scope="row">Response</th>
		<td><details open="open">
				<summary>Status line and header section (890 bytes)</summary>
				
				<pre><code>HTTP/1.1 200 OK
Access-Control-Allow-Origin: *
Content-Length: 28
Content-Type: text/plain; charset=utf-8
Date: Wed, 12 Aug 2026 19:28:49 GMT
Etag: W/&quot;1c-8HgF6mNyhsSFK0pascC9uB0wjX0&quot;
Feature-Policy: payment &#39;self&#39;
Nel: {&quot;report_to&quot;:&quot;heroku-nel&quot;,&quot;response_headers&quot;:[&quot;Via&quot;],&quot;max_age&quot;:3600,&quot;success_fraction&quot;:0.01,&quot;failure_fraction&quot;:0.1}
Report-To: {&quot;group&quot;:&quot;heroku-nel&quot;,&quot;endpoints&quot;:[{&quot;url&quot;:&quot;https://nel.heroku.com/reports?s=XJ%2BSWZNyTjZrXjtc88LQgL%2FIIBv5MD1mBAlUm4hYMKE%3D\u0026sid=812dcc77-0bd0-43b1-a5f1-b25750382959\u0026ts=1786562929&quot;}],&quot;max_age&quot;:3600}
Reporting-Endpoints: heroku-nel=&quot;https://nel.heroku.com/reports?s=XJ%2BSWZNyTjZrXjtc88LQgL%2FIIBv5MD1mBAlUm4hYMKE%3D&amp;sid=812dcc77-0bd0-43b1-a5f1-b25750382959&amp;ts=1786562929&quot;
Server: Heroku
Vary: Accept-Encoding
Via: 1.1 heroku-router
X-Content-Type-Options: nosniff
X-Frame-Options: SAMEORIGIN
X-Recruiting: /#/jobs

</code></pre>
				
				
			</details> <details class="response-body" open="open">
				<summary>Response body (28 bytes)</summary>
				
				<pre><code>User-agent: *
Disallow: /ftp</code></pre>
				
				
			</details></td>
	</tr>
	<tr>
		<th scope="row">Parameter</th>
		<td><pre><code>cache-control</code></pre></td>
	</tr>
	
	
	<tr>
		<th scope="row">Solution</th>
		<td> 
<p>For secure content, ensure the cache-control HTTP header is set with &quot;no-cache, no-store, must-revalidate&quot;. If an asset should be cached consider setting the directives &quot;public, max-age, immutable&quot;.</p>
 </td>
	</tr>
</table>

											</details></li>
									</ol>
								</li>
								
							</ol>
						</li>
						
					</ol>
				</li>
				  
			</ol>
		</section>

		<section id="appendix" class="appendix">
			<h2>Appendix</h2>

			<section id="alert-types" class="alert-types">
				<h3>Alert Types</h3>
				<p class="alert-types-intro">This section contains additional information on the types of alerts in the report.</p>
				<ol>
					<li
						id="alert-type-0">
						<h4>Configuração Incorreta Entre Domínios</h4>
						<table class="alert-types-table">
							<tr>
								<th scope="row">Source</th>
								<td>
									
									   <span>raised by a passive scanner</span> <span>(<a
										href="https://www.zaproxy.org/docs/alerts/10098/">Configuração Incorreta Entre Domínios</a>)
									</span>   
								</td>
							</tr>
							<tr>
								<th scope="row">CWE ID</th>
								<td><a
									href="https://cwe.mitre.org/data/definitions/264.html">264</a></td>
							</tr>
							<tr>
								<th scope="row">WASC ID</th>
								<td>14</td>
							</tr>
							<tr>
								<th scope="row">Reference</th>
								<td>
									<ol>
										<li><a
											href="https://vulncat.fortify.com/en/detail?category=HTML5&amp;subcategory=Overly%20Permissive%20CORS%20Policy">https://vulncat.fortify.com/en/detail?category=HTML5&amp;subcategory=Overly%20Permissive%20CORS%20Policy</a></li>
									</ol>
								</td>
							</tr>
						</table>
					</li>
					<li
						id="alert-type-1">
						<h4>Content Security Policy (CSP) Header Not Set</h4>
						<table class="alert-types-table">
							<tr>
								<th scope="row">Source</th>
								<td>
									
									   <span>raised by a passive scanner</span> <span>(<a
										href="https://www.zaproxy.org/docs/alerts/10038/">Content Security Policy (CSP) Header Not Set</a>)
									</span>   
								</td>
							</tr>
							<tr>
								<th scope="row">CWE ID</th>
								<td><a
									href="https://cwe.mitre.org/data/definitions/693.html">693</a></td>
							</tr>
							<tr>
								<th scope="row">WASC ID</th>
								<td>15</td>
							</tr>
							<tr>
								<th scope="row">Reference</th>
								<td>
									<ol>
										<li><a
											href="https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/CSP">https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/CSP</a></li>
										<li><a
											href="https://cheatsheetseries.owasp.org/cheatsheets/Content_Security_Policy_Cheat_Sheet.html">https://cheatsheetseries.owasp.org/cheatsheets/Content_Security_Policy_Cheat_Sheet.html</a></li>
										<li><a
											href="https://www.w3.org/TR/CSP/">https://www.w3.org/TR/CSP/</a></li>
										<li><a
											href="https://w3c.github.io/webappsec-csp/">https://w3c.github.io/webappsec-csp/</a></li>
										<li><a
											href="https://web.dev/articles/csp">https://web.dev/articles/csp</a></li>
										<li><a
											href="https://caniuse.com/#feat=contentsecuritypolicy">https://caniuse.com/#feat=contentsecuritypolicy</a></li>
										<li><a
											href="https://content-security-policy.com/">https://content-security-policy.com/</a></li>
									</ol>
								</td>
							</tr>
						</table>
					</li>
					<li
						id="alert-type-2">
						<h4>Divulgação de Data e Hora - Unix</h4>
						<table class="alert-types-table">
							<tr>
								<th scope="row">Source</th>
								<td>
									
									   <span>raised by a passive scanner</span> <span>(<a
										href="https://www.zaproxy.org/docs/alerts/10096/">Divulgação de Data e Hora</a>)
									</span>   
								</td>
							</tr>
							<tr>
								<th scope="row">CWE ID</th>
								<td><a
									href="https://cwe.mitre.org/data/definitions/497.html">497</a></td>
							</tr>
							<tr>
								<th scope="row">WASC ID</th>
								<td>13</td>
							</tr>
							<tr>
								<th scope="row">Reference</th>
								<td>
									<ol>
										<li><a
											href="https://cwe.mitre.org/data/definitions/200.html">https://cwe.mitre.org/data/definitions/200.html</a></li>
									</ol>
								</td>
							</tr>
						</table>
					</li>
					<li
						id="alert-type-3">
						<h4>Strict-Transport-Security Header Not Set</h4>
						<table class="alert-types-table">
							<tr>
								<th scope="row">Source</th>
								<td>
									
									   <span>raised by a passive scanner</span> <span>(<a
										href="https://www.zaproxy.org/docs/alerts/10035/">Strict-Transport-Security Header</a>)
									</span>   
								</td>
							</tr>
							<tr>
								<th scope="row">CWE ID</th>
								<td><a
									href="https://cwe.mitre.org/data/definitions/319.html">319</a></td>
							</tr>
							<tr>
								<th scope="row">WASC ID</th>
								<td>15</td>
							</tr>
							<tr>
								<th scope="row">Reference</th>
								<td>
									<ol>
										<li><a
											href="https://cheatsheetseries.owasp.org/cheatsheets/HTTP_Strict_Transport_Security_Cheat_Sheet.html">https://cheatsheetseries.owasp.org/cheatsheets/HTTP_Strict_Transport_Security_Cheat_Sheet.html</a></li>
										<li><a
											href="https://owasp.org/www-community/Security_Headers">https://owasp.org/www-community/Security_Headers</a></li>
										<li><a
											href="https://en.wikipedia.org/wiki/HTTP_Strict_Transport_Security">https://en.wikipedia.org/wiki/HTTP_Strict_Transport_Security</a></li>
										<li><a
											href="https://caniuse.com/stricttransportsecurity">https://caniuse.com/stricttransportsecurity</a></li>
										<li><a
											href="https://datatracker.ietf.org/doc/html/rfc6797">https://datatracker.ietf.org/doc/html/rfc6797</a></li>
									</ol>
								</td>
							</tr>
						</table>
					</li>
					<li
						id="alert-type-4">
						<h4>Modern Web Application</h4>
						<table class="alert-types-table">
							<tr>
								<th scope="row">Source</th>
								<td>
									
									   <span>raised by a passive scanner</span> <span>(<a
										href="https://www.zaproxy.org/docs/alerts/10109/">Modern Web Application</a>)
									</span>   
								</td>
							</tr>
							
							
							
						</table>
					</li>
					<li
						id="alert-type-5">
						<h4>Re-examine Cache-control Directives</h4>
						<table class="alert-types-table">
							<tr>
								<th scope="row">Source</th>
								<td>
									
									   <span>raised by a passive scanner</span> <span>(<a
										href="https://www.zaproxy.org/docs/alerts/10015/">Re-examine Cache-control Directives</a>)
									</span>   
								</td>
							</tr>
							<tr>
								<th scope="row">CWE ID</th>
								<td><a
									href="https://cwe.mitre.org/data/definitions/525.html">525</a></td>
							</tr>
							<tr>
								<th scope="row">WASC ID</th>
								<td>13</td>
							</tr>
							<tr>
								<th scope="row">Reference</th>
								<td>
									<ol>
										<li><a
											href="https://cheatsheetseries.owasp.org/cheatsheets/Session_Management_Cheat_Sheet.html#web-content-caching">https://cheatsheetseries.owasp.org/cheatsheets/Session_Management_Cheat_Sheet.html#web-content-caching</a></li>
										<li><a
											href="https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Headers/Cache-Control">https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Headers/Cache-Control</a></li>
										<li><a
											href="https://grayduck.mn/2021/09/13/cache-control-recommendations/">https://grayduck.mn/2021/09/13/cache-control-recommendations/</a></li>
									</ol>
								</td>
							</tr>
						</table>
					</li>
				</ol>
			</section>
		</section>
		 
	</main>
</body>
</html>



