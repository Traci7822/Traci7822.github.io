---
layout: post
title:  "HTTP request to Rails DB & Angular Views"
date:   2016-11-26 14:26:41 -0500
---


This is the path that I took to populate a Rails database with data parsed from an external $http request and present it through Angular on the front end...

I am using data from data.colorado.gov to populate a database of Colorado Alternative Fuel Stations (https://dev.socrata.com/foundry/data.colorado.gov/c7ve-fkni)

//RAILS

First create a rake task to get the data:

> rails g task import_stations 

Then build out the rake task to retrieve and parse the data then create Rails objects with the data:


> */lib/tasks/import_stations.rake*
> 
> require 'net/http'
> 
> namespace :import do
>   desc "import fuel station database"
>   task :stations => :environment do
>     uri = URI.parse('https://data.colorado.gov/resource/c7ve-fkni.json')
>     res = Net::HTTP.get(uri)
>     response = JSON.parse(res)
>     response.each do |r|
>       Station.create(ev_connector_types: r['ev_connector_types'], fuel_type_code: r["fuel_type_code"], name: r["station_name"], street_address: r["street_address"], phone: r["station_phone"], hours: r["access_days_time"], station_id: r["id"], city: r["city"], state: r["state"], zip: r["zip"])
>     end
>   end
> end

Over in my StationsController I have my create method:

> */app/controllers/stations_controller.rb*
> 
>   def create
>     @station = Station.new(station_params)
>     if @station.save
>       render json: @station, status: :created,
>       location: @station
>     else
>       render json: @station.errors, status: :unprocessable_entity
>     end
>   end
> 	
> 	private
> 	
> 	def station_params
>     params.require(:station).permit(:id, :fuel_type_code, :name, :street_address, :phone, :hours, :station_id, :city, :state, :zip, :ev_connector_types)
>   end

I serialize the data with ActiveModel:Serializer

> */app/serializers/station_serializer.rb*
> 
> class StationSerializer < ActiveModel::Serializer
>   attributes :id, :fuel_type_code, :name, :street_address, :phone, :hours, :station_id, :city, :state, :zip, :ev_connector_types
> end

Now the json data is viewable at 

> *http://localhost:3000/stations.json*
> 
 If you don't already have it, the JSONview chrome extension makes json data really nice and readable.  
 
//ANGULAR

First I set my route/state in the module:

> */app/assets/javascripts/app.js*
> 
> angular
>   .module('app', ['ui.router', 'templates', 'Devise'])
>   .config(function($stateProvider, $urlRouterProvider) {
>     $stateProvider
>       .state('stations', {
>         url: '/stations',
>         templateUrl: 'stations/stations.html',
>         controller: 'StationController as vm',
>       })  
>   })

Then build a service which will get the json data that I want to show:

> */app/assets/javascripts/app/services/StationService.js*
> 
> function StationService($http) {
>   this.getStations = function () {
>     return $http.get('/stations.json')
>   }
> }
> 
> angular
>   .module('app')
>   .service('StationService', StationService);

Then I use the service within my controller:

> */app/assets/javascripts/app/controllers/StationController.js*
> 
> function StationController($scope, StationService){
>   StationService
>     .getStations()
>     .then(function (res) {
>       $scope.stationList = res.data;
>     })
> }
> 
> angular
>   .module('app')
>   .controller('StationController', StationController);

And finally in my view, I output the data:

> */app/assets/javascripts/app/stations/stations.html*
>  
>  
> <div ng-controller="StationController">
> 	<table>
> 		<th>City</th>
> 		<th>Name</th>
> 		<th>Phone #</th>
> 		<th>Address</th>
> 		<th>Business Hours</th>
> 		<th>EV Connector Types</th>
> 		<th>Fuel Type</th>
> 		<tr ng-repeat="station in stationList">
> 			<td>{{station.city}}</td>
> 			<td>{{station.name}}</a></td>
> 			<th>{{station.phone}}</th>
> 			<th>{{station.street_address}}</th>
> 			<td>{{station.hours}}</td>
> 			<td>{{station.ev_connector_types}}</td>
> 			<td>{{station.fuel_type_code}}</td>
> 		</tr>
> 	</table>
> </div>


was a tough one to figure out but a neat way to learn how rails and angular can work together using external resources!



