<?php
/*
copyright @ bncautowork.com
Modified by Ilyasa
And Modified by BillieDhikaS
2017
*/
require_once('./line_class.php');

$channelAccessToken = 'y5OVSLv5M35qlNZLTg0OLJQH81id/qGMHREizJGvvnQnuU7V8dbWD6jw31pcrznA6R1MEd9aVP0lIOsTM1lj9G1G97pXzoN5SOPfw5duiIgkVGDcYKu7m9FSz+LlT2oVHo0UMapj5iEkymRATJwvTQdB04t89/1O/w1cDnyilFU='; //Your Channel Access Token
$channelSecret = '200a64520445dbf90c137ead511293b3';//Your Channel Secret

$client = new LINEBotTiny($channelAccessToken, $channelSecret);

$userId 	= $client->parseEvents()[0]['source']['userId'];
$replyToken = $client->parseEvents()[0]['replyToken'];
$message 	= $client->parseEvents()[0]['message'];
$profil = $client->profil($userId);
$pesan_datang = $message['text'];

if($message['type']=='sticker')
{	
	$balas = array(
							'UserID' => $profil->userId,	
                                                        'replyToken' => $replyToken,							
							'messages' => array(
								array(
										'type' => 'text',									
										'text' => 'jngn pake sticker dehh.'										
									
									)
							)
						);
						
}
else
$pesan=str_replace(" ", "%20", $pesan_datang);
$key = 'cd969571-764d-4e6d-9577-3b980c73bfcc'; //API SimSimi
$url = 'http://sandbox.api.simsimi.com/request.p?key='.$key.'&lc=id&ft=1.0&text='.$pesan;
$json_data = file_get_contents($url);
$url=json_decode($json_data,1);
$diterima = $url['response'];
if($message['type']=='text')
{
if($url['result'] == 404)
	{
		$balas = array(
							'UserID' => $profil->userId,	
                                                        'replyToken' => $replyToken,													
							'messages' => array(
								array(
										'type' => 'text',					
										'text' => 'Pake bahasa yg bener donggg :D.'
									)
							)
						);
				
	}
else
if($url['result'] != 100)
	{
		
		
		$balas = array(
							'UserID' => $profil->userId,
                                                        'replyToken' => $replyToken,														
							'messages' => array(
								array(
										'type' => 'text',					
										'text' => 'Sorry Coy '.$profil->displayName.' gw lg sibuk nih.'
									)
							)
						);
				
	}
	else{
		$balas = array(
							'UserID' => $profil->userId,
                                                        'replyToken' => $replyToken,														
							'messages' => array(
								array(
										'type' => 'text',					
										'text' => ''.$diterima.''
									)
							)
						);
						
	}
}
 
$result =  json_encode($balas);

file_put_contents('./reply.json',$result);


$client->replyMessage($balas);
