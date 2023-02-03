For the first question, I decided to find the mean of the star type column as shown here:

      print("The most common star type is "+str(thestars["Star type"].mean())+" which are main sequence stars.")

img1 shows the result

For the second question, I found a correlation between temperatures and star color as shown here

      thestars.drop(thestars[thestars['Radius(R/Ro)'] > 2000].index, inplace = True)
      thestars.drop(thestars[thestars['Radius(R/Ro)'] < 1000].index, inplace = True)
      thestars.plot(kind = 'scatter', x = "Temperature (K)", y = "Star color")
      plt.show()
      print("As we can see, stars with lower temperatures are of hotter colors and stars with higher temperatures are of cooler colors\n")

img2 shows the result

For the third question, I found that luminosity is most influential in classifying a stars type.

      print("This dataset depics the relationship between luminosity and the type of star")
      newstars = pd.read_csv(temp)
      newstars.plot(kind = 'scatter', x = "Luminosity(L/Lo)", y = "Star type")
      plt.show()
      print("Here we can see a correlation with how how bright the star is and the type of star.\n")

img3 shows the result

For the fourth question, this is how I made my diagram by using the Ballesteros formula and plotting the values on a scatterplot with temperature on the x axis
and luminocity on the y axis

      print("This is a Hertzsprung-Russell Diagram.")
      newstars.loc[newstars["Spectral Class"] == "M", "Spectral Class"] = 1.4
      newstars.loc[newstars["Spectral Class"] == "K", "Spectral Class"] = .81
      newstars.loc[newstars["Spectral Class"] == "G", "Spectral Class"] = .58
      newstars.loc[newstars["Spectral Class"] == "F", "Spectral Class"] = .3
      newstars.loc[newstars["Spectral Class"] == "A", "Spectral Class"] = -.02
      newstars.loc[newstars["Spectral Class"] == "B", "Spectral Class"] = -.3
      newstars.loc[newstars["Spectral Class"] == "O", "Spectral Class"] = -.33

      newstars["Temperature (K)"] = ( 1/(newstars["Spectral Class"]*.92 + 0.62) + 1/(.92*newstars["Spectral Class"] + 1.7)) * 4600


      starclass = ["   O   ", "   B   ", "   A   ", "   F   ", "    G   ", "   K   ", "   M   "]
      width = 500
      height = 700
      dots = 100
      fig, graph = plt.subplots(figsize = (width/dots, height/dots))

      graph.scatter(newstars["Temperature (K)"], newstars["Luminosity(L/Lo)"], s = 0.5, c="k")
      graph.set_yscale("log")
      graph.set_xscale("log")
      graph.set_xlim(40000, 3000)
      graph.set_ylim(1.e-5, 1.e6)
      graph.set_xticks([30000, 15000, 10000, 5000, 3000])
      graph.set_ylabel("Luminosity (Solar Units)")
      graph.set_xlabel("Temperature (K)")
      graph.get_xaxis().set_major_formatter(ScalarFormatter())
      top = graph.secondary_xaxis('top')
      top.set_xlabel(starclass)
      plt.show()
      
img4 shows the result      
 
For the fifth question, I trained a machine learning model to predict star type by using linear regression with numpy like so.

    stararray = newstars[["Temperature (K)", "Luminosity(L/Lo)", "Radius(R/Ro)", "Absolute magnitude(Mv)", "Spectral Class"]].to_numpy()
    typearray = newstars[["Star type"]].to_numpy()
    starcolor = newstars[["Star color"]].to_numpy()

    stararray,typearray = np.array(stararray), np.array(typearray)

    reg_model = LinearRegression().fit(stararray, typearray)

    result = reg_model.score(stararray, typearray)
    print("This machine learning model has "+str(round(result*100, 2))+" percent accuracy.")

    prediction = reg_model.predict(stararray)
    print("It will now predict each star type for each row:\n "+str(prediction)+"\n")

img5 shows the result

For the bonus question, I found the row our sun was in by taking known values and subtracting them by their respective elements and taking the absolute values.
I then add all elements and the one that is closest to zero is the row that closley resembles our sun.

      sunseeker = pd.read_csv(temp)
      newstararray = sunseeker["Temperature (K)"]

      itr = 0
      winningindex = 0
      winner = 1000
      for i in prediction:    
          if i>=2.5 and i<3.5:

              runningcolor = str(starcolor[itr])    
              if runningcolor[2] == "y" or runningcolor[2] == "Y":           
                  runningluminocity= abs(stararray[itr, [1]]-1)
                  runningtemp = abs(newstararray[itr] - 6000)
                  runningradius = abs(stararray[itr, [2]] - 1)
                  runningabsmagnitude = abs(stararray[itr, [3]] + 26.74)
                  runningsun = runningabsmagnitude + runningluminocity + runningradius + runningtemp
                  if winner > runningsun:
                      runningsun = winner
                      winningindex = itr

          itr +=1

      print("The row that most closley resembles our sun is this one:\n")
      print(sunseeker.iloc[winningindex,:])
      
      
The result is shown in img6
