---
layout: post
title: Regular Expressions
image: /img/lukas_voit/Profil_Voit.JPG
---

Western:

I decided only to do the western part as i´m completly lost with the arabic alphabet.

Part 1:
[e]|ate?(s|n)?(ing)?(ry)?r?

Part 2. Find all Qadhdhafis:
\b((M[uao`']+mm?[ae]r?) ([aAeE][lI][- ])?([GKQ][h]?[au][dthz']+[a?]f+[i?y]))\b

Part 3:
\b([IiEe][?s][bfp]|[SsHh][ei][sp]+|[Nn]esf-e [Jj])([a?]h[?a]n)\b

Conversion 1:

[A]skin\,[\s]Leon$
Leon Askin

2. Austrian Cities
Vienna|Graz|Linz|Salzburg|Innsbruck|Klagenfurt|Villach|Wels|Sankt Pölten|Dornbirn|Wiener Neustadt|Steyr|Feldkirch|Bregenz|Leonding|Klosterneuburg|Baden bei Wien|Wolfsberg|Leoben|Krems|Traun|Amstetten|Lustenau|Kapfenberg|Mödling|Hallein|Kufstein|Traiskirchen|Schwechat|Braunau am Inn|Stockerau|Saalfelden|Ansfelden|Tulln|Hohenems|Spittal an der Drau|Telfs|Ternitz|Perchtoldsdorf|Feldkirchen|Bludenz|Bad Ischl|Eisenstadt|Schwaz|Hall in Tirol|Gmunden|Wörgl|Wals-Siezenheim|Marchtrenk|Bruck an der Mur|Sankt Veit an der Glan|Korneuburg|Neunkirchen|Hard|Vöcklabruck|Lienz|Rankweil|Hollabrunn|Enns|Brunn am Gebirge|Ried im Innkreis|Bad Vöslau|Waidhofen|Knittelfeld|Trofaiach|Mistelbach|Zwettl|Völkermarkt|Götzis|Sankt Johann im Pongau|Gänserndorf|Gerasdorf bei Wien|Ebreichsdorf|Bischofshofen|Groß-Enzersdorf|Seekirchen am Wallersee|Sankt Andrä


3. ([^,]([\w ]*))(?=( \(Lower Austria\)))

   ([^,]([\w ]*))(?=( \(Salzburg\)))|(Wals-Siezenheim)

